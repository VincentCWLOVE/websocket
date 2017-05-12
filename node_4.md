# Socket.IO

Socket.io是目前最流行的WebSocket实现，包括服务器和客户端两个部分。它不仅简化了接口，使得操作更容易，而且对于那些不支持WebSocket的浏览器，会自动降为Ajax连接，最大限度地保证了兼容性。它的目标是统一通信机制，使得所有浏览器和移动设备都可以进行实时通信。

Socket.IO设计的目标是构建能够在不同浏览器和移动设备上良好运行的实时应用，如实时分析系统、二进制流数据处理应用、在线聊天室、在线客服系统、评论系统、WebIM等。目前，Socket.IO已经支持主流PC浏览器(如IE、Safari、Chrome、Firefox、Opera等)和移动平台上的浏览器（iOS平台下的Safari、Android平台下的基于Webkit的浏览器等）。

Socket.IO已经具有众多强大功能的模块和扩展API，如（session.socket.io)（http session中间件，进行session相关操作）、socket.io-cookie（cookie解析中间件）、session-web-sockets（以安全的方式传递Session）、socket-logger（JSON格式的记录日志工具）、websocket.MQ（可靠的消息队列）、socket.io-mongo（使用MongoDB的适配器）、socket.io-redis（Redis的适配器）、socket.io-parser（服务端和客户端通讯的默认协议实现模块）等。

Socket.IO实现了实时、双向、基于事件的通讯机制,它解决了实时的通信问题，并统一了服务端与客户端的编程方式。启动了Socket以后，就像建立了一条客户端与服务端的管道，两边可以互通有无。它还能够和Express.js提供的传统请求方式很好的结合，即可以在同一个域名，同一个端口提供两种连接方式。

[Socket.IO 官网](https://socket.io/)

### 开始编程

> 创建一个项目文件夹`socket_server`，打开控制台

```bash

	cd 项目文件夹路径/socket_server
	npm init

```

> 使用编辑器(sublime Text),打开项目文件夹`socket_server`，选中`package.json`文件，键入如下代码：

```json

  "dependencies": {
  	"express": "*",
  	"socket.io": "*"
  }

```

> 切换到控制台，键入

```bash

	npm install

```

> 安转依赖完成以后，切换到编辑器，键入以下代码：

```javascript

	var app = require('express')();
	var http = require('http').Server(app);
	var io = require('socket.io')(http);

	app.get('/', function(req, res){
		res.send('<h1>Welcome to Vincent'Server</h1>');
	});

	//在线用户
	var onlineUsers = {};
	//当前在线人数
	var onlineCount = 0;

	io.on('connection', function(socket){
		console.log('a user connected');
		
		//监听新用户加入
		socket.on('login', function(obj){
			//将新加入用户的唯一标识当作socket的名称，后面退出的时候会用到
			socket.name = obj.userid;
			
			//检查在线列表，如果不在里面就加入
			if(!onlineUsers.hasOwnProperty(obj.userid)) {
				onlineUsers[obj.userid] = obj.username;
				//在线人数+1
				onlineCount++;
			}
			
			//向所有客户端广播用户加入
			io.emit('login', {onlineUsers:onlineUsers, onlineCount:onlineCount, user:obj});
			console.log(obj.username+'加入了聊天室');
		});
		
		//监听用户退出
		socket.on('disconnect', function(){
			//将退出的用户从在线列表中删除
			if(onlineUsers.hasOwnProperty(socket.name)) {
				//退出用户的信息
				var obj = {userid:socket.name, username:onlineUsers[socket.name]};
				
				//删除
				delete onlineUsers[socket.name];
				//在线人数-1
				onlineCount--;
				
				//向所有客户端广播用户退出
				io.emit('logout', {onlineUsers:onlineUsers, onlineCount:onlineCount, user:obj});
				console.log(obj.username+'退出了聊天室');
			}
		});
		
		//监听用户发布聊天内容
		socket.on('message', function(obj){
			//向所有客户端广播发布的消息
			io.emit('message', obj);
			console.log(obj.username+'说：'+obj.content);
		});
	  
	});

	http.listen(3000, function(){
		console.log('listening on *:3000');
	});

```

