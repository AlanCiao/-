# XML 基础

[TOC]

## 概述





## 语法





## 约束

DTD

元素：<!ELEMENT 元素名称 使用规则>

使用规则：

​	EMPTY

​	ANY

​	(...) 

​		若干 元素/#PCDATA

​			空格分隔：无序

​			逗号分隔：有序

​			竖线分隔：选择一个

​		+，?，*：规定个数

​		()：用来分组



属性：<!ATTLIST 元素名

​		属性名1 属性类型 设置说明

​		属性名1 属性类型 设置说明

​		……

​	    >

属性类型：

​	CDATA

​	(ENUMERATED1 | ENUMERATED2 | ...)

​	ID (一般设为#REQUIRED 不能重复)

​	IDREF IDREFS (类似于外键)

​	NMTOKEN NMTOKENS (用前一个给定标识，用后一个批量引用)

​	NOTATION 值 (声明类型或处理程序)

​		<!NOTATION 值 SYSTEM "MIME"/"URL">

​	ENTITY ENTITYS (表名属性值需要使用引用实体)

设置说明：

​	#REQUIRED

​	#IMPLIED

​	#FIXED <默认值>

​	<默认值>



实体：

​	引用实体：（在 XML 中被引用）

​		<!ENTITY 实体名称 "实体内容">

​		<!ENTITY 实体名称 SYSTEM "外部 XML 文档的 URL">

​		引用方法：&实体名称;

​	参数实体：（在 DTD 中自己使用）

​		<!ENTITY % 实体名称 "实体内容">

​		引用方法：%实体名称;



DTD 混合内容？ #PCDATA 和其他元素

只能声明为 * ，#PCDATA必须放在第一位？

Schema

http://www.w3.org/2001/XMLSchema-instance



#### 名称空间

与标签不同，没有前缀的属性不属于任何名称空间。



名称空间可以绑定空字符串，表示不属于任何一个名称空间。

## 解析

由解析器对 XML 文档进行解析，而一般程序员的工作是编写程序调用解析器的 API 获取 XML 文档的内容。解析器就是一个对应借口的实现类，一般由解析器开发者完成。

两套 XML 解析器的标准：

DOM Document Object Model W3C标准

SAX Simple API for XML 民间标准

解析器就是这些标准/规则的实现类

为了屏蔽实现类的不同厂商，Sun 公司制定了 JAXP Java API for XML Processing 规范，该规则也是一些借口，封装了不同解析器实现类，以至于可以使用同样的接口来使用它们。



JAXP包：javax.xml org.w3c.dom org.xml.sax

JAXP 就是将解析器的创建进行封装，使用者不需要知道具体的解析器是什么，而只要使用 JAXP 标准就可以了。



