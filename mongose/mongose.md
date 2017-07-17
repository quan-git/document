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
```
# 原型
```
原理是一个json格式的类, 这个类包含一些关于文档的类型, 属性等信息. 原型可以作为文档的蓝图. 模型创建的时候需要原型. 
所以在我们使用模型的属性前, 需要先定义它们的原型.

例子:
var bookSchema = mongoose.Schema({
  name: String
})
原型的类型:
1.String: 字符串类型
2.Number: 数字类型
3.Date: 日期类型
4.Buffer: 二进制类型
5.Boolean: 布尔类型
6.Schema.Types.Mixed: 任何类型的数据
7.Schema.Types.ObjectId: MongoDB中一个典型的24个字符, 12字节的十六进制的数字字符串.
8.Array: 数组类型
```
# 模型
```
模型是最基础的对象,将原型编译为一个模型:
var Book=mongoose.model('Book',bookSchema)
模型的静态方法:
Model.find(query,[fields],[iptions],[calback(error,docs)])//查找与查询条件
Model.remove(query, [callback(error)]) //删除集合中与查询条件匹配的文档.
```
# 案例
```
var mongoose = require('mongoose');
var url = 'mongodb://192.168.1.11:27017/stu';

mongoose.connect(url);              //链接数据库
var db = mongoose.connection;       

db.on('error', function (error) {  //发生错误时的事件
  console.log(error);
});

db.once('open', function(){         //数据库打开时发生的事件
  console.log('connect is OK.');
});

var Schema = mongoose.Schema;
var schema = new Schema({          //做一个原型
  name: String,
  binary:  Buffer,
  living:  Boolean,
  updated: { type: Date, default: Date.now },
  age:     { type: Number, min: 18, max: 65 },
  mixed:   Schema.Types.Mixed,
  _someId: Schema.Types.ObjectId,
  array:      [],
  ofString:   [String],
  ofNumber:   [Number],
  ofDates:    [Date],
  ofBuffer:   [Buffer],
  ofBoolean:  [Boolean],
  ofMixed:    [Schema.Types.Mixed],
  ofObjectId: [Schema.Types.ObjectId],
  nested: {
    stuff: { type: String, lowercase: true, trim: true }
  }
});
// example use
var Info = mongoose.model('Info', schema);  //作出模型
var m = new Info;
m.name = 'Statue of Liberty'
m.age = 25;
m.updated = new Date;
m.binary = new Buffer(0);
m.living = false;
m.mixed = { any: { thing: 'i want' } };
m.markModified('mixed');
m._someId = new mongoose.Types.ObjectId;
m.array.push(1);
m.ofString.push("strings!");
m.ofNumber.unshift(1,2,3,4);
m.ofBuffer.pop();
m.ofMixed = [1, [], 'three', { four: 5 }];
m.nested.stuff = 'good';

m.save(function(err, data){
  console.log('save ok.');
});
```


