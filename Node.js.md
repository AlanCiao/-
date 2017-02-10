# Node.js

简单的说， Node.js 就是运行在服务端的 JavaScript 。它是一个事件驱动 I/O 服务端 JavaScript 环境，基于 Google 的V8 引擎。就是用它可以运行 JavaScript 脚本，是 JavaScript 的运行环境（目前的理解）。

Node.js 可以实现整个 HTTP 服务器。

一个简单的 HTTP 服务器：

```javascript
// server.js
var http = require('http');

http.createServer(function (request, response) {
  response.writeHead(200, {'Content-Type': 'text/plain'});
  response.end('Hello World\n');
}).listen(8888);

console.log('Server running at http://127.0.1:8888');
```



Node.js 提供了一个简单的模块系统，一个  Node.js 文件就是一个模块。

exports 是模块公开的借口，require 用于从外部获取一个模块的借口。这是两个对象，用来提供借口和导入借口（exports 对象）。