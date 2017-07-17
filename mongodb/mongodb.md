# 安装
```
1. $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
2. $ echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
3. $ sudo apt-get update
4. sudo apt-get install -y mongodb-org
```
# 启动
```
1. $ sudo service mongod start
1. $ mongo
```
# 数据库的操作
1. 创建 
```
use 数据库名字  //如果这个数据库有就切换到这个数据库上如果没有就会创建一个新的
```
2. 查询
```
 db //查看当前所在的数据库 
 show dbs //查看所有的数据库注只有写入数据的数据库才会显示
 ```
 3. 删除
 ```
db.dropDatabase()
```
# 集合的操作
1. 集合的创建
```
db.collectionName.insert();
```
2. 集合的删除
```
db.collectionName.drop();
```
3. 集合查询
```
db.collectionName.find();
```
# 文档的操作
1. 插入文档
```
db.collectionName.insert({"name":"lisi"});
db.collectionName.insert(try)   //try是一个保存键值对的变量
如果指定_id字段用save()方法,如果不指定save()和insert()的效果一样
```
2. 更新文档
```
db.collectionName.update()
db.collectionName.save()
db.collectionName.update(
   { query},   //update的查询条件。
   {update},   //update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
   {
     upsert: <boolean>,   //可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入
     multi: <boolean>,    //可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新
     writeConcern: <document> //可选，抛出异常的级别  
   }
)
//更新第一条符合条件
db.one.update({"name":"zhangsan"}, {$set:{"age": 30}})

//更新所有符合条件
db.one.update({"name":"zhangsan"}, {$set:{"age": 30}}, false,true)

//没有符合条件的,添加一条文档进去
db.one.update({"name":"zhangsan"}, {$set:{"age": 30}}, true,true)

//将所有name=zhangsan的文档的age增加5  
db.one.update({"name":"zhangsan"}, {$inc:{"age": 5}}, false,true)

//将所有name=zhangsan的文档的age和course字段删除   
db.one.update({"name":"zhangsan"}, {$unset: {"age":1, "course":1}}, false,true)

//在一个数组域中添加一个数据并且将size域的值加１
db.one.update({"age":10}, {$push:{"course":"html"}, $inc:{"size": 1}}, false, false)

//在一个数组域中添加多个数据
db.one.update({"age":10}, {$pushAll:{"course":["html", "python"]}}, false, false)

//删除数组域中的一个数据
db.firstClass.update({"age":20},{$pop:{"course":-1}},false,false) //删除course第一个
db.firstClass.update({"age":20},{$pop:{"course":1}},false,false) //删除course最后一个

//删除数组域中的某个特定数据
db.firstClass.update({"age":20}, {$pull: {"course": "python"}}, false,false)

//删除数组域中的多个数据
db.firstClass.update({"age":20}, {$pullAll: {"course": ["python", "c"]}}, false,false)

//修改某个域的名字
db.firstClass.update({"age":20}, {$rename: {"course": "class"}}, false,false)

//在某个数组域中添加数据(数据不存在时，才会添加)
db.firstClass.update({"age":20}, {$addToSet: {"course": "python"}}, false,true)
```
3. 删除文档
```
db.collectionName.remove(
   {query},    //删除指定条件的文档　
   {
     justOne: boolean,   // 设置为true或者１时，仅仅删除第一个符合条件的文档, 默认删除所有符合条件的
     writeConcern: document   //抛出异常　
   }
)
```
4. 查找文档
```
查找方法：　
db.collectionName.find()
db.collectionName.findOne()

查找所有文档:
db.collectionName.find()

按照条件操作符查找：　　　　

db.collectionName.find({"age": {$lt: 10}})   //　查询年龄小于　10的文档
db.collectionName.find({"age": {$lte: 10}})   //　查询年龄小于等于　10的文档
db.collectionName.find({"age": {$gte: 10}})   //　查询年龄大于等于　10的文档
db.collectionName.find({"age": {$gt: 10}})   //　查询年龄大于　10的文档
db.collectionName.find({"age": {$ne: 10}})   //　查询年龄不等于　10的文档
db.collectionName.find({"age": {$mod: [10,0]}})   //　查询年龄能被10整除文档
db.collectionName.find({"age": {$not: {$mod: [10,0]}}})// 查询年龄不能被10整除的文档
db.collectionName.find({"course": {$all: ["english", "math"]}})  //查询course包含english和math的文档
db.firstClass.find({"course": {$size : 3}})  //查找　course数组元素个数等于３的文档

   
按照 and 条件查询：同时满足所有条件　　　

db.collectionName.find({"name":"lisi", "age":10})  // 查找name="lisi", age=10的文档
db.collectionName.find({"name":"lisi", "age":10}).pretty()  //按照合适的格式显示　　

 

按照　or 条件查询: 满足其中的一个条件即可　　　

db.collectionName.find({$or: [{"age":10}, {"age": 11}]}).pretty() //查找age=10 or age=11的文档

and 和　or 条件混合使用：　　　　　

//查找　name=lisi and (age=10 or age=11)的文档
db.collectionName.find({"name":"lisi", $or:[{"age":10},{"age": 11}]}).pretty()

控制查询到的文档的数量：limit()
db.collectionName.find().limit(2) //输出查询的到的前两条文档

跳过查询到的前几条文档,输出其他文档：skip()
db.collectionName.find().skip(1) //跳过前１条文档，从后边开始显示

统计查询到的文档个数：　count()
db.collectionName.find().count()

对查询结果进行排序：　　　　　　
db.collectionName.find().sort({"age": 1}) //按照age值进行升序排列
db.collectionName.find().sort({"age": -1}) //按照age值进行降序排列

将查询到的文档按照指定的域输出：　　　
db.collectionName.find({}, {"name": 1, "age": 1}) //查询到的文档，仅仅输出name和age两个域
```
# 远程服务器
1. 安装mongodb包
2. 链接数据库
```
var MongoClient = require('mongodb').MongoClient;
var assert = require('assert');

var url = 'mongodb://127.0.0.1:27017/chat';
MongoClient.connect(url, function(err, db) {
  assert.equal(null, err);
  console.log('Connected correctly to server.');
  db.close();
});
```
3. 插入数据
```
var mydbInit = function(db, callback) {
   db.collection('restaurants').insertOne( {
      "address" : {
         "street" : "2 Avenue",
         "zipcode" : "10075",
         "building" : "1480",
         "coord" : [ -73.9557413, 40.7720266 ]
      },
      "borough" : "Manhattan",
      "cuisine" : "Italian",
      "grades" : [
         {
            "date" : new Date("2014-10-01T00:00:00Z"),
            "grade" : "A",
            "score" : 11
         },
         {
            "date" : new Date("2014-01-16T00:00:00Z"),
            "grade" : "B",
            "score" : 17
         }
      ],
      "name" : "Vella",
      "restaurant_id" : "41704620"
   }, function(err, result) {
    assert.equal(err, null);
    console.log("Inserted a document into the restaurants collection.");
    callback();
  });
};


MongoClient.connect(url, function(err, db) {
  assert.equal(null, err);
  console.log('Connected correctly to server.');

  mydbInit(db, function(){
      console.log("init OK.");
      db.close();
  });
});
```
4. 查询操作
```
var MongoClient = require('mongodb').MongoClient;
var assert = require('assert');
var url = 'mongodb://mydb:asdfgh@ds151137.mlab.com:51137/mydb';
var findDB = function(db, callback){
    var cursor = db.collection('restaurants').find();
    cursor.each(function(err, doc){
        assert.equal(err, null);
        if(doc !== null){
            console.dir(doc);
        } else {
            callback();
        }
    });
};

MongoClient.connect(url, function(err, db) {
  assert.equal(null, err);
  console.log('Connected correctly to server.');

  findDB(db, function(){
      db.close();
  });
});

查询方法
var cursor =db.collection('restaurants').find( { "borough": "Manhattan" } );  指定一个查询条件的例子
var cursor =db.collection('restaurants').find( { "address.zipcode": "10075" } );  指定窃套属性
var cursor =db.collection('restaurants').find( { "grades.score": { $gt: 30 } } ); 查询大于
var cursor =db.collection('restaurants').find( { "grades.score": { $lt: 10 } } )  查询小于
var cursor =db.collection('restaurants').find(        //AND运算
  { "cuisine": "Italian", "address.zipcode": "10075" }
);

var cursor =db.collection('restaurants').find(      // or运算
  { $or: [ { "cuisine": "Italian" }, { "address.zipcode": "10075" } ] }
);
```
5. 数据更新
```
var MongoClient = require('mongodb').MongoClient;
var assert = require('assert');

var url = 'mongodb://mydb:asdfgh@ds151137.mlab.com:51137/mydb';

var updateDB = function(db, callback){
    db.collection('restaurants').updateOne(
        {"name": "Juni"},   //匹配条件
        { $set: { "cuisine": "China",  //更新数据项
				  "restaurant_id": "555555", 
				  "lastModified": true } 
		},
      function(err, results) {    //第三参数回调函数
      console.log(results);
      callback();
  });
};

MongoClient.connect(url, function(err, db) {
  assert.equal(null, err);
  console.log('Connected correctly to server.');

  updateDB(db, function(){
      db.close();
  });
});
```
6. 删除数据
```
删除符合条件的数据
var MongoClient = require('mongodb').MongoClient;
var assert = require('assert');

var url = 'mongodb://mydb:asdfgh@ds151137.mlab.com:51137/mydb';

var removeDB = function(db, callback) {
   db.collection('restaurants').deleteMany(
      { "name": "Vella" },
      function(err, results) {
         console.log(results);
         callback();
      }
   );
};

MongoClient.connect(url, function(err, db) {
  assert.equal(null, err);
  console.log('Connected correctly to server.');
  removeDB(db, function(){
      db.close();
  });
});

删除符合条件的第一条数据:
db.collection('restaurants').deleteOne(
      { "borough": "Queens" },
      function(err, results) {
         console.log(results);
         callback();
      }
   );
   
删除所有文档:
db.collection('restaurants').deleteMany( {}, function(err, results) {
      console.log(results);
      callback();
   });
   
删除整个集合:
db.collection('restaurants').drop( function(err, response) {
      console.log(response)
      callback();
   });
```
