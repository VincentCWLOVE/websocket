# 服务器需求分析

`webSocket`协议是专门用于解决 `Web`和 `server`的长链接的协议，即服务器要同时维持大量连接处于打开状态。那么服务器的架构需要是能以低性能开销接收高并发数据的架构。要求无非是以下的任意一点：

> 围绕线程

> 非阻塞 IO

### 推荐服务器开发语言及框架选择

> Node.js

	Socket.IO
	WebSocket-Node
	ws

> Java

	Jetty

> Ruby

	EventMachine

> Python
	
	pywebsocket
	Tornado

> Erlang

	Shirasu

> C++
	
	libwebsockets

> .NET

	SuperWebSocket

