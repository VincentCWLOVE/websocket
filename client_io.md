
### 关键代码

```javascript

	//连接websocket后端服务器
	this.socket = io.connect('websocket服务地址');

	//告诉服务器端有用户登录
	this.socket.emit('login', {});

	//监听新用户登录
	this.socket.on('login', function(o){
		
	});

	//监听用户退出
	this.socket.on('logout', function(o){
		
	});

	//监听消息发送
	this.socket.on('message', function(obj){
	
	});

```