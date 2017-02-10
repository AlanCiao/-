## HTTP 协议

HTTP 是无状态协议，每一次的请求都是独立的，不会对后面的请求和相应造成影响。

HTTP 1.1 相对于 HTTP 1.0 支持可持续连接，在一次连接中可以完成多次请求。



通用消息头：

Cache-Control 缓存的使用和处理

Connection: Keep-Alive | Close  HTTP 1.1 默认 Keep-Alive

Date 事件，GMT 格式

Pragma: no-cache

Trailer 声明的消息头可以放到内容之后出现

Transfer-Encoding: chunked 消息分段传输，不需要使用 Content-Length 了

Upgrade 更新协议 101 响应

Via 标记中介代理服务器和协议

Warning 附加警告信息



请求头：

Accept 可以接收的 MIME 类型

Accept-Charset 可接收的字符集

Accept-Encoding: gzip, compress 可接受的数据压缩方式

Accept-Language 期待接收的语言

Authorization 验证消息

Proxy-Authorization 针对代理的验证

Expect: 100-continue  询问服务器是否可以发送一个附加文档

From 邮件地址

Host 请求的主机

If-Match 实体内容是否包含某些字段，用来检测请求的文档是否改变

If-None-Match 

If-Modified-Since GET 方式，通过时间判断文件是否改变，如果未改变，返回 304 

If-Unmodified-Since 只用于 PUT 方式

If-Range 与 Range 一同使用，通过 Range 的设置判断是否更新文件

Range 请求文档的一个部分，用于断点续传

Max-Forwards 最大代理数

Referer 前一个连接地址

TE: trailers, deflate 支持的扩展传输编码类型

User-Agent 



响应头：

Accept-Range: none | bytes

Age 指定文件缓存时间

Etag 实体标签，给客户端用来判断文件是否改变

Location 重定向地址

Retry-After 与 503 结合使用

Server 服务器软件名称

Vary 使服务器收到影响的请求头，用来检查是否需要发送新请求，如果改变就发请求而不从缓存中获取

WWW-Authenticate 结合 401 使用，要求用户提供验证信息， realm 指定一个域，该验证对一个域中的所有信息都试用

Proxy-Authenticate 针对代理服务器的用户信息认证



实体头：

Allow 伴随 405 指定资源所支持的请求方法

Content-Encoding 指定压缩类型

Content_Language 指定返回文档的语言

Content-Length 表示实体内容的长度（字节数），适用于 POST、PUT 和 DELETE

Content-Location 内容的实际 URL

Content-MD5 提供内容完整性检查

Content-Range 

Content-Type 内容的 MIME 类型

Expires 设置 GMT 格式的时间，设为 0 等非正确格式，表示立刻过期

Last-Modified GMT 格式，指定文档最后更改时间



扩展头：

Refresh: 1; [URL]  过多少秒后自动刷新页面

Content-Disposition: attachment; filename=file.zip 添加附件，可以下载