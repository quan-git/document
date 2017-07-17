# 简介
```
Mongoose是一个提供了MongoDB地相映射的Node.js库，它在Node.js中与ORM（Object Relational Mapping）有着类似的接口。
如果你不熟悉ORM或者Mongoose中的Object Data Mapping（ODM），意思是Mongoose将数据库中的数据转换为JavaScript对象以
供你在应用中使用。多种中间件可以用于连接node.js与MongoDB，目前比较常用的Mongoose
```
# 使用
```
1. 例子
运行这个脚本必须保证mongodb处于运行中
var mongoose = require('mongoose');     //引入mongoose的模块
var url = 'mongodb://mydb:asdfgh@ds151137.mlab.com:51137/mydb'; //mongodb数据库地址
mongoose.connect(url);    //和Mongodb建立链接

var db = mongoose.connection;
db.on('error', function(){        //错误的监听函数
    console.log("Connect error");
});

db.once('open', function(){       //打开的监听函数
    console.log("mongoose working!!!");
});

2. schema
