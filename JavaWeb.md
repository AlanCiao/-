## JavaWeb

### TOMCAT

一般 startup.bat 需要 JAVA_HOME 配置， startup.bat 启动 catalina.bat ，并还要运行 setclasspath.bat 文件。



虚拟目录映射

虚拟目录映射类似于路由，虚拟目录优先于真实目录，采用最长路径匹配原则——先查询最后一个目录对应的映射（或路由），如果存在就直接访问，而不继续查询，如果不存在就向上一级目录，然后查询该目录下面是否存在子目录，以此类推。



虚拟主机

一台服务器 IP 对应多个虚拟主机名称，这时只能用主机名/域名来访问这个网站，而不能用 IP 来直接访问了，因为服务器不知道到底要访问哪个虚拟主机。



Tomcat 可以通过 JK 插件并采用 AJP 协议通信与其他 Web 服务器进行集成。



Tomcat 中的 Servlet 引擎调用 Servlet 是采用多线程方式，为每一个请求创建一个新的线程来调用 Servlet 的 service 方法。因此要注意线程安全问题。



ServletConfig 中不但封装了 Servlet 初始化参数，还封装的 Servlet 容器对象，即 ServletContext 。GenericServlet 已经实现 ServletConfig 接口。



缺省 Servlet 会检查之前是否调用过 getWriter 方法，如果没有调用，则调用 getOutputStream 方法，如果调用了，则直接使用 getWriter 。因为这两个不可用同时被调用。缺省 Servlet 有缓存页面一小段时间的效果。



当使用 Last-Modified 响应头，这样通过链接访问页面的时候，浏览器在本次运行时就不会向服务器发送请求，一般适用于静态页面；当不使用 Last-Modified 响应头时，浏览器每次都会向服务器发送请求，一般使用于动态页面。

刷新是一定会向服务器发出请求的。

前进后退直接读取缓存文件，而不发送请求，但如果不存在缓存则发送请求。

超链接要检查文件是否有 Last-Modified 来决定是否发出请求。

在次强调一下，如果请求的文件包含 Last-Modified 头，那么浏览器默认本次运行都不会再向服务器发出请求。但是可以通过刷新功能强制发出请求，请求头中会带上 If-Modified-Since 头，然后根据服务器得到的 Last-Modified 值判断是否要返回请求资源，如果不需要，即修改时间没有变，那么就返回 304 ，让浏览器读取缓存。



日志记录：

在 server.xml 中加入日志标签：

<Logger className="org.apache.catalina.logger.FileLogger" directory="logs" prefix="localhost_log." suffix=".txt" timestamp="true" />



Tomcat 应该看做是一个包含独立 Servlet 容器的服务器，它本身可以以插件的形式与 IIS 或 Apache 等 Web 服务器进行连接交互。同时，Tomcat 自身也是一个 Web 服务器，但是它处理静态页面的能力不如上述大型的 Web 服务器。



路径匹配，扩展匹配，默认匹配

Tomcat 中 Servlet 的虚拟路径就是用来进行路由匹配的。



Tomcat 中 org.apache.catalina.session.StandardManager 和 org.apache.catalina.session.PersistentManager 实现 Session 持久化功能。

自定义 Session 管理配置：

<Context path="..." docBase="...">

​	<Manager className="..." ...>

​		<Store className="..." .../>  // 一个存储驱动类

​	</Manager>

</Context>



### Cookie 和 Session

小知识：一般规定一个浏览器只能存放 300 个 Cookie ，一个网站最多存放 20 个 Cookie ，一个 Cookie 不能超过 4KB 。如果没有设置过期时间或设为负数，则 Cookie 只在内存中存储，当关闭浏览器之后， Cookie 就消失了，如果设置为零，则立即删除该 Cookie 。



Session 防表单重复提交，在表单中放入一个隐藏字段，设置一个唯一标号 token ，并将这个唯一标号存在一个 Session 中，当表单被提交时，比较提交的 token 与 Session 中存放的 token 是否一致，如果一致则允许提交，并且提交后删除 Session 中的 token ，当表单被重复提交时，就无法找到 token ，那么该提交的表单就会被忽略。



### JSP

JSP 的 out 内置对象是 JSPWriter 类型，是一个包裹了 PrintWriter 的包装类，内部自带一个缓存，使用 out 写入的文本内容会先存入这个缓存中，最后， out 会调用 getWriter 方法把缓存的内容写入到响应中。所以要特别注意不要同时出现 getOutputStream 方法的调用。

PageContext 对象封装了目前 JSP 页面中所有的信息，包括页面配置属性和隐式对象，以及一些常用的方法。

pageContext.forward() 方法在转发前，会清空 out 缓冲区里的所有内容，而 RequestDispatcher 会清空 Servlet 引擎中缓冲区的内容，但不会对 out 缓冲区起作用。



统一设置 JSP 页面的默认编码（相当于 pageEncoding ）：

web.xml

```xml
<jsp-config>
  <jsp-property-group>
    <url-pattern>/url</url-pattern>
    <page-encoding>pageEncoding</page-encoding>
  </jsp-property-group>
</jsp-config>
```



### JavaBean

JSP 中提供了专门处理 Javabean 的动作标签。

<jsp:useBean id="beanInstanceName" class="package.class" scope="page | request | session | application" [type="package.class" beanName="package.class"] />

class 和 beanName 不能同时出现，因为功能类似。type 必须和 beanName 同时出现。有 class 时 type 默认与之相同，可以省略。

<jsp:setProperty name="beanInstanceName" property="propertyName" [value="String/expression" | param="parameterName"] />

这个标签如果嵌套在前一个标签中的话，那么相当于新建 JavaBean 对象时进行初始化。

<jsp:getProperty name="beanInstanceName" property="PropertyName" />



### EL 表达式

忽略 EL 表达式，但最终会以页面显示设置的值为准。

```xml
<!-- web.xml -->
<jsp-config>
  <jsp-property-group>
    <url-pattern>*.jsp</url-pattern>
    <!-- 忽略 EL 表达式 -->
    <el-ignored>true</el-ignored>
    <!-- 禁用 JSP 脚本 -->
    <scripting-invalid>true</scripting-invalied>
  </jsp-property-group>
</jsp-config>
```

EL 的隐含对象：





## JavaWeb 知识体系

1. XML
   - 语法
   - DTD 约束
   - Schema 约束
2. Tomcat
   - Web 应用
   - Tomcat 配置和工作原理
3. HTTP 协议
   - 请求行和状态行
   - 请求头和响应头（包括通用信息头，实体头和扩展头等）
4. Servlet
   - 概念及原理
   - ServletConfig
   - GenericServlet 与 HttpServlet
   - 浏览器缓存问题
   - ServletContext
   - 记录日志
   - 访问资源文件
   - Web 应用程序之间的访问
5. HttpServletResponse
   - 响应状态行
   - 响应消息头
   - 中文输出、定时刷新/跳转、禁止浏览器缓存
   - 响应正文 getOutputStream 和 getWriter
   - 文件下载，访问计数
   - 重定向与转发
6. HttpServletRequest
   - 获取请求行和网络连接信息 API
   - 获取请求头信息
   - Referer 防盗链，隐藏 JS 源代码，客户端身份认证
   - JS 防止表单重复提交
   - 获取请求参数和实体内容
   - 文件上传
   - Request 域
   - 中文参数问题：URL 编码
7. Session
   - Cookie
   - Session 跟踪机制：JSESSIONID，Cookie 和 URL 重写
   - Session 域
   - 购物车，防止表单重复提交 token ，验证码，上次访问时间
   - Session 持久化
8. JSP
   - 本质
   - 9 大内置对象
   - 基本语法
   - JSP 指令
   - pageContext 对象
   - JSP 标签
   - 中文乱码问题
9. JavaBean
   - 概念
   - JSP 中三个有关 JavaBean 的标签（这些内容实际上已经不重要了）
   - MVC 设计模式





高级部分：

1. ​
2. EL 表达式
3. ​
4. ​