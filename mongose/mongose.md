# 简介
```
Mongoose是一个提供了MongoDB地相映射的Node.js库，它在Node.js中与ORM（Object Relational Mapping）有着类似的接口。
如果你不熟悉ORM或者Mongoose中的Object Data Mapping（ODM），意思是Mongoose将数据库中的数据转换为JavaScript对象以
供你在应用中使用。多种中间件可以用于连接node.js与MongoDB，目前比较常用的Mongoose
```
# 使用
```
1. 链接
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
mongoose.Schema方法用来定义数据集的格式（schema），mongoose.model方法将格式分配给指定的数据集。
Schema仅仅只是一断代码，他书写完成后程序依然无法使用，更无法通往数据库端, 它仅仅只是数据库模型在程序
片段中的一种表现，或者是数据属性模型.
Schema、Model、Entity的关系请牢记，Schema生成Model，Model创造Entity， Model和Entity都可对数据
库操作造成影响，但Model比Entity更具操作性。

定义一个schema
var Schema = mongoose.Schema;
var userSchema = new Schema({
    name: String,
    age: Number,
    DOB: Date,
    isAlive: Boolean
});

将schema发布为model,这个模型变量名的首字母一定要大写
var User = mongoose.model('User', userSchema);

用model创建Entity
var arvind = new User({
    name : 'David',
    age: 23,
    DOB: '01/01/1999',
    isAlive: true
});

使用Entity储存数据库
arvind.save(function(err, data){

})

用Model进行查询
User.findOne({}, function(err, data){
    console.log(data);
})
