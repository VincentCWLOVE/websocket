# ws/wss

### `http vs ws` / `https vs wss`

> WebSocket协议用`ws`表示,`wss`表示加密的WebSocket协议,类似于 `http/https`

	ws://example.com/wsapi
	wss://secure.example.com/

> websocket使用和 http 相同的 TCP 端口,即 80 端口

> wws默认使用`443`端口

> 和 `http` 协议一样， `websocket`也具有请求的消息格式和响应的消息的格式

```bash
#客户端请求
GET / HTTP/1.1
Upgrade: websocket
Connection: Upgrade
Host: example.com
Origin: http://example.com
Sec-WebSocket-Key: sN9cRrP/n9NdMgdcy2VJFQ==
Sec-WebSocket-Version: 13

```

```bash
#服务器端响应
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: fFBooB7FAkLlXgRSz0BT3v4hq5s=
Sec-WebSocket-Location: ws://example.com/

```

> 字段说明
1. Connection必须设置Upgrade，表示客户端希望连接升级。
2. Upgrade字段必须设置Websocket，表示希望升级到Websocket协议。
3. Sec-WebSocket-Key是随机的字符串，服务器端会用这些数据来构造出一个SHA-1的信息摘要。把“Sec-WebSocket-Key”加上一个特殊字符串“258EAFA5-E914-47DA-95CA-C5AB0DC85B11”，然后计算SHA-1摘要，之后进行BASE-64编码，将结果做为“Sec-WebSocket-Accept”头的值，返回给客户端。如此操作，可以尽量避免普通HTTP请求被误认为Websocket协议。
4. Sec-WebSocket-Version 表示支持的Websocket版本。RFC6455要求使用的版本是13，之前草案的版本均应当被弃用。
5. Origin字段是可选的，通常用来表示在浏览器中发起此Websocket连接所在的页面，类似于Referer。但是，于Referer不同的是，Origin只包含了协议和主机名称。
6. 其他一些定义在HTTP协议中的字段，如Cookie等，也可以在Websocket中使用。


### 最重要的一点是：WebSocket协议没有“同域限制”。