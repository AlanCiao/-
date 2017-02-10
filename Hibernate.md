# Hibernate

## Hibernate 入门

### 概述

Hibernate ORM 是完成实体对象到关系型数据库表之间相互映射的一种工具，它可以自动实现这些操作，而不需要考虑不同的数据库和 SQL 语句。

#### 实体

首先，对应每一张表，都要有一个实体对象，这些对象应当遵循标准 JavaBean 规范，将所有属性私有化，并且提供 getter 和 setter 方法。还需要提供无参构造器，便于用反射创建对象。

#### 映射

Hibernate 采用 XML 文件来配置实体和数据表之间的映射关系，可以参考 DTD 文件来了解元素的种类和方法，一个基本的映射如下所示。

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN" "http://hiberante.sourceforge.net/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
  <class name="className" table="tableName">
    <id name="id属性" coloumn="主键字段">
      <generator class="native" />
    </id>
    <property name="其他属性名" type="类型" column="字段" />
    <property name="..." />
    ...
  </class>
</hibernate-mapping>
```

property 中的 name 属性对应实体中的一对 getter 和 setter ，如果没有给出 column 属性，表示表中的字段与 name 同名。type 属性指定了 Java 类型和数据库数据类型之间的转换关系，例如日期可以指定 `type="timestamp"` 来表示将 java.util.Date 转换为时间戳存储，如果未指定类型， Hibernate 会尝试自动转换。

映射文件习惯以 `.hbm.xml` 为结尾，放在与实体的源文件相同的目录下。

#### 数据库连接配置

普遍使用 XML 配置文件，格式如下。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuation PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN" "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
  <session-factory>
    <!-- Database connection settings -->
    <property name="connection.driver_class">数据库驱动</property>
    <property name="connection.url">连接url</property>
    <property name="connection.username">username</property>
    <property name="connection.password">password</property>
    
    <!-- JDBC connection pool(use the build-in) -->
    <property name="connection.pool_size">1</property>
  
    <!-- SQL dialect -->
    <property name="dialect">方言名称</property>
  
    <!-- Enable Hibernate's automatic session context management -->
    <property name="current_session_context_class">thread</property>
  
    <!-- Disable the second-level cache -->
    <property name="cache.provider_class">org.hibernate.cache.NoCacheProvider</property>
  
    <!-- Echo all executed SQL to stdout -->
    <property name="show_sql">true</property>
  
    <!-- Drop and re-create the database schema on startup -->
    <property name="hbm2ddl.auto">create</property>
  
    <!-- 为持久化类加入映射文件 -->
    <mapping resource="*.hbm.xml" />
  </session-factory> 
</hibernate-configuration>
```

这个配置关联一个 SessionFactory ，一个 SessionFactory 对应一个数据库的连接配置，该文件应当放到 classpath 的根目录下。通过 SessionFactory 可以获得一个 Session ， Session 与数据库事务相对应。





## 官方文档学习 (5.0.7)

### 结构

Hibernate 是进行 ORM 映射的框架，它在应用程序和数据库之间搭建起一座桥梁，将持久化对象映射到数据库表中，需要的是两种配置文件来进行配置，分别是主配置文件 hibernate.cfg.xml (hibernate.properties) 和映射文件 *.hbm.xml 。

几个主要的类：

- SessionFactory 线程安全的工厂，包含所需的配置信息，用来创建 Hibernate Session ，极其耗费资源，所以一个应用中仅创建一个即可。
- Session 线程相关的短生命周期的工作单元，是 java.sql.Connection 的包装类，可以用来生成 Transaction ，是操作数据持久化的主要对象。
- Transaction 线程相关的短生命周期对象，用来划定事务的范围。

Contextual sessions

与 getCurrentSession() 相关的内容，用于提供获取同一个 session 。 Hibernate 将这一功能做成 plugin 的形式，提供一个 org.hibernate.context.spi.CurrentSessionContext 接口和一个 hibernate.current_session_context_class 属性来实现这个插件。 Hibernate 为这个接口提供了三个实现类，最常用的就是 ThreadLocalSessionContext 类，通过 ThreadLocal 来实现同一上下文 session 的获取，配置参数为 "Thread" ，这个参数就是用来指定所使用的实现类的。

session 的生命周期与事务绑定，一个事务对应一个 session 。

### 域模型 (Domain Model)

Domain Model 也被称作 persistent classes ，域模型组成了所有需要映射的类， Hibernate 使用（单不强制） Plain Old Java Object (POJO) / JavaBean 模型。

POJO Domain Models ：

一些基本的定义

- 需要有空参构造器
- 需要提供表示属性 Identifier attribute(s)
- 建议非 final 类和 getter/setter 方法（这里是为了创建动态模型功能）
- 为持久化属性声明 getters 和 setters
- 重写 equals() 和 hashCode() 方法
  - 最后一点有些 ambiguous ，并非所有的实体类都需要重写这两个方法，只有使用复合 identifiers 和与集合有关的对象时，才需要指定这两个方法。
  - 在 Hibernate 中，尽量保持同样的数据记录只对应一个实体类的对象，它们都应该是相同的，同一个 session 中， Hibernate 通过一级缓存来保证同一个对象，但是当瞬时态或游离态的对象存入集合中，需要确保它们的独立性，那么就要设置该实体类的 equals() 和 hashCode() 方法。
  - 在实现这两个方法时，也要注意如何生成 hashCode ，如果它是与 id 依赖的，那么要注意放入集合和设定 id 的顺序即它们之间的关系（具体可参考文档范例）。

动态模型 Dynamic models

Hibernate 允许动态生成实体类对象，只需要有映射文件，而不需要创建实体类，便可以在程序运行时由 Hibernate 自动生成相应的实体对象。尽管动态模型无法进行编译时检查，但是它能很好的统一实体类型的和数据库表的格式。

### 启动 Native Bootstraping

#### ServiceRegistry

用来配置服务注册信息，如果使用默认值（大部分情况），则可以跳过这些配置。







##### 问题：

什么是 JTA ？

如何理解 JPA ？

Java Persistent API 好像是一个 Java 接口规范