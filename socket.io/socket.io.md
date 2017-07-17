# 实际作用
```
socket.io 是一个为实时应用提供跨平台实时通信的库。socket.io 旨在使实时应用在每个浏览器和移动设备上成为可能，
模糊不同的传输机制之间的差异。socket.io 是一个以实现跨浏览器、跨平台的实时应用为目的的项目。针对不同的浏览器
版本或者不同客户端会做自动降级处理，选择合适的实现方式（websocket, long pull..），隐藏实现只暴露统一的接口。
可以让应用只关注于业务层面上。
```
# 例子
```
var app = require('express')();     
var server = require('http').Server(app);
var io = require('socket.io')(server);  //使socket.io绑定到服务器上任何链接到服务器客户端都具备了实时通信功能
server.listen(3000);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){       //这里connection事件监听所有客户端并返回该并返回新的链接对象
  console.log("a user connected!");
  socket.emit('news', {hello: 'world'});
  socket.on('other', function(data){
    console.log(data);
  });
});
socket.io提供了三种默认事件(客户端和服务器端都有):
1.当与对方链接后自动触发connect 
2.收到对方发来的数据时触发message事件通常为socket.send()触发
3.当对方关闭链接触发disconnect事件

在服务器端区分以下三种情况:
1.socket.emit():向建立链接的客户端广播
2.socket.broadcast.emit():向除去建立该连接的客户端所有客户广播
3.io.sockets.emit():向所有客户端广播,等同于上面俩个和

客户端:
引用socket.io.js实现
如果是与本地服务器链接var socket=io.connect('http://localhost')
如果是与其他服务器链接var socket=io.connect(服务器地址);
如果客户端和服务器位于同一个服务器上  var socket=io();
```
# demo
```
服务器:
var io = require('socket.io').listen(80);

io.sockets.on('connection', function (socket) {
  socket.on('ferret', function (name, fn) {
    fn('woot');
  });
});

客户端:
<script>
  var socket = io(); // TIP: io() with no args does auto-discovery
  socket.on('connect', function () { // 你可以避免监听“连接”，也可以直接听事件! 
    socket.emit('ferret', 'tobi', function (data) {
      console.log(data); // data will be 'woot'
    });
  });
</script>
```
