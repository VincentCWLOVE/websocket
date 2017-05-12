# 浏览器端对WebSocket协议的处理

> 建立连接和断开连接<br>

```javascript

	//客户端要检查浏览器是否支持WebSocket
	if(window.WebSocket != undefined) {

		// Create WebSocket connection.
		var socket = new WebSocket('ws://localhost:3000');

		// Connection opened
		socket.addEventListener('open', function (event) {
		    socket.send('Hello Server!');
		});

		// Listen for messages
		socket.addEventListener('message', function (event) {
		    console.log('Message from server', event.data);
		});

		//有些错误发生
		socket.addEventListener('error', function (err) {
		    console.log(err);
		});
	}

```



> 发送数据和接收数据<br>

```javascript

	socket.send("发送文本");

	// 使用ArrayBuffer发送canvas图像数据
	var img = canvas_context.getImageData(0, 0, 400, 320);
	var binary = new Uint8Array(img.data.length);
	for (var i = 0; i < img.data.length; i++) {
		binary[i] = img.data[i];
	}
	socket.send(binary.buffer);

	// 使用Blob发送文件
	var file = document.querySelector('input[type="file"]').files[0];
	socket.send(file);

```



