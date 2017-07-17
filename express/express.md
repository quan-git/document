# Express
```
Express.js 是基于Node.js中HTTP模块和Connect组件的WEB框架, 这些组件叫作中间件, 它们是以
约定大于配置原则作为开发的基础理念的.
```
# Express安装
```
$ npm i express-generator -g  //全局安装
$ npm i express --save        //项目里安装
```
快速创建一个express项目
```
$ express -e demo  //-e添加EJS引擎支持,默认下使用jade
```
# 创建模块
```
创建一个启动文件index.js
var express = require('express');
var app = express();
app.use(express.static(__dirname + '/public')); //设置静态文件
app.listen(3000)
这个时候访问服务器会访问public文件目录下的index.html 也可以有文件夹和其它资源
```
# 路由器
> 实际情况会把路由放到一个单独的文件中
```
module.exports = function(app){
  app.get('/', function(req, res){
    res.send('Weclome to Node.js.');
  });

  app.get('/doc', function(req, res){
    res.send('doc page');
  });

  app.get('/admin', function(req, res){
    res.send('admin page');
  });
};
```
> 使用
```
var express = require('express');
var app = express();
var routes = require('./routes/route.js')(app);
app.listen(3000);
```
> 路由的正则
```
app.get('/ab?cd', function(req, res) {
  res.send('ab?cd');
});

app.get('/ab+cd', function(req, res) {
  res.send('ab+cd');
});
```
# 响应方法
```
res.download()	  提示将要下载文件。
res.end()	        结束响应进程。
res.json()	      发送 JSON 响应。
res.jsonp()	      在 JSONP 的支持下发送 JSON 响应。
res.redirect()	  重定向请求。
res.render()	    呈现视图模板。
res.send()	      发送各种类型的响应。
res.sendFile	    以八位元流形式发送文件。
res.sendStatus()	设置响应状态码并以响应主体形式发送其字符串表示
```
# 中间件
```
中间件（middleware）就是处理HTTP请求的函数。它最大的特点就是，一个中间件处理完，再传递给下一个中间件。
实例在运行过程中，会调用一系列的中间件。
```
# 模板引擎
> 设置模板的渲染和目录
```
app.set("views", __dirname + "/views");   //t方法用于指定变量的值
app.set("view engine", "ejs");
```
# 编写启动脚本
> 新建项目目录
```
一般都使用模板
$ express -e demo //-e是使用ejs

```
# 配置路由
> 不使用ejs
```
app.get('/', function(req, res) {
   res.sendfile('./views/index.html');
});
```
>使用ejs模板
```
app.set('view engine', 'ejs');
app.set('views', __dirname+'/views');
app.get('/', function (req, res){
	res.render('index', {name: 'express'});
});
```
> 变量/页面方法
```
<%=name %>    //用于变量赋值
<% include ./footer %>  //用于引入其他ejs文件
```
>forEach使用
```
路由:
app.get('/about', function(req, res) {
  var info = [{name: 'Mary', age: 20},
  {name: 'Ben', age: 32},
  {name: 'Scotch', age: 21}
];
	res.render('about', {
    info: info,
    title: "Information"
  });
});
页面处理数据:
<div>
  <h2> <%= title %> </h2>
  <ul>
    <% info.forEach(function(info){ %>
      <div>
  		<li><b>name:</b><%= info.name %></li>
  		<li><b>age:</b><%= info.age %></li>
	</div>
    <% }); %>
  </ul>
</div>
```
# get/post请求
> get请求
```
app.get('/about', function(req, res) {
  var info = [{name: 'Mary', age: 20},
  {name: 'Ben', age: 32},
  {name: 'Scotch', age: 21}
];
console.log(req.query.name);  //get请求直接访问req的query就可以拿到传来的数据
	res.render('about', {     //渲染并返回信息
    info: info,
    title: "Information"
  });
});
```
> post请求
```
1. 首先安装
$ npm install body-parser --save
2.使用插件
app.use(bodyParser.json({limit: '1mb'}));  //body-parser 解析json格式数据
app.use(bodyParser.urlencoded({            //此项必须在 bodyParser.json 下面,为参数编码
  extended: true
}));
3.req.body可接收到所收到的数据
```






