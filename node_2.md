# Node 服务器端编程

做过服务器端编程的童鞋，都了解过， `ASP`、`ASP.NET`需要`IIS`作为服务器，`PHP`需要`Apache`或`Nginx`环境，`JSP`需要`Tomcat`,事实上使用`Node`，可以很方便的去构建一个服务器，不需要而外的容器，这一点挺方便的。

`Node`提供了 `net` `dgram` `http` `https` 四个模块，分别处理 `TCP` `UDP` `HTTP` `HTTPS` 等网络协议


### 构建 `TCP`服务

```javascript

	var net = require("net");

	var server = net.createServer(function(socket){
		//接收从客户端传过来的信息
		socket.on('data',function(data){
			socket.write(data.toString());
		});

		socket.on('end',function(){
			console.log("断开了链接");
		});

		socket.on('error',function(err){
			console.log(err);
		})

		socket.write("Welcome to Vincent`s server");

		socket.pipe(socket);
	});
	server.linsten(3300,function(){
		
	})
```
[了解更多]("http://www.runoob.com/nodejs/nodejs-net-module.html")

### 构建 `HTTP`服务

```javascript

	var http = require('http');
	var fs = require('fs');
	var url = require('url');


	// 创建服务器
	http.createServer( function (request, response) {  
	   // 解析请求，包括文件名
	   var pathname = url.parse(request.url).pathname;
	   
	   // 输出请求的文件名
	   console.log("Request for " + pathname + " received.");
	   
	   // 从文件系统中读取请求的文件内容
	   fs.readFile(pathname.substr(1), function (err, data) {
	      if (err) {
	         console.log(err);
	         // HTTP 状态码: 404 : NOT FOUND
	         // Content Type: text/plain
	         response.writeHead(404, {'Content-Type': 'text/html'});
	      }else{	         
	         // HTTP 状态码: 200 : OK
	         // Content Type: text/plain
	         response.writeHead(200, {'Content-Type': 'text/html'});	
	         
	         // 响应文件内容
	         response.write(data.toString());		
	      }
	      //  发送响应数据
	      response.end();
	   });   
	}).listen(8081);

	// 控制台会输出以下信息
	console.log('Server running at http://127.0.0.1:8081/');
```
[了解更多]("http://www.runoob.com/nodejs/nodejs-web-module.html")