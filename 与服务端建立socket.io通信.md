# 与服务端建立socket.io通信

### server (nod, koa2)
 安装socket.io ``` npm i socket ```

 ```
const Koa = require('koa');
const app = new Koa();
const server = require('http').createServer(app.callback())
const io = require('socket.io')(server);

server.listen(3000);

io.on('connection', function (socket) {
  socket.emit('news', { hello: 'world' });
  socket.on('hello', function (data) {
    console.log(data);
  });
});
 
 ```
 
 
### client
 安装socket.io  ``` npm i socket-client ```
  ```
 import io from 'socket.io-client'

const socket = io.connect('http://localhost:3000', {
  transports: ['websocket', 'polling']
})
socket.on('connect', function (data) {
  console.log('data', data)
})
socket.on('news', function (data) {
  console.log('news: ', data)
  socket.emit('hello', 'helloworld')
})

  ```
