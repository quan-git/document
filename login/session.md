# bcrypt算法
```
bcrypt算法是由美国著名的密码学专家、美国军方密码标准参与人布鲁斯·施内尔在1993年发布的Blowfish加密算法演化而来。
bcrypt验证方式和其它加密方式不同，不是直接解密得到明文，也不是二次加密比较密文，而是把明文和存储的密文一块运算得
到另一个密文，如果这两个密文相同则验证成功。
```
# 安装
```
$ npm install --save bcrypt
```
# 案例
```
数据库配置文件
const mongoose = require('mongoose');
const url = 'mongodb://mydb:asdfgh@ds151702.mlab.com:51702/mydb';

mongoose.connect(url);

let db = mongoose.connection;

db.once('open', function(){
  console.log('connect db ok!');
})

let Schema = mongoose.Schema;

let userShema = Schema({
  name: {type: String},
  password: {type: String}
});


module.exports.user = mongoose.model('login', userShema);
```

```
login页面的路由处理程序:
var express = require('express');
var bcrypt = require('bcrypt');
var router = express.Router();
var db = require('../db');
var salt = 10;

router.get('/', (req, res, next) => {
  res.render('login', {title: 'Login'});
});

router.post('/', (req, res, next) => {
  console.log(req.body);

  db.user.findOne({
    name: req.body.name
  }, function(err, data) {
    bcrypt.compare(req.body.password, data.password, function(err, hash) {
      if (hash) {
        res.redirect('/');
      } else {
        res.redirect('/login');
      }
    });
  });
});

module.exports = router;
```


```
页面路由文件处理程序
var express = require('express');
var bcrypt = require('bcrypt');
var router = express.Router();
var db = require('../db');
var salt = 10;

router.get('/', (req, res, next) => {
  res.render('register', {title: 'register'});
});

router.post('/', (req, res, next) => {
  bcrypt.hash(req.body.password, salt, (err, hash) => {
    var user = new db.user({
      name: req.body.name,
      password: hash
    });
    user.save((err, data) => {
      res.redirect('/');
    });
  });

});

module.exports = router;
```

# session
> 安装
```
npm i express-session --save
```
> 例子
```
app.use(session({
  resave: true,
  saveUninitialized: false,
  secret: '3nqr9xzx2438fgsdam4324n',
  cookie: {
      maxAge: 1000 * 60 * 30
  }
}));


跟cookie一样都需要单独的安装和引用模块， 安装模块：$sudo npm install express-session 主要的方法就是 session(options)，其中 options 中包含可选参数，主要有：
name: 设置 cookie 中，保存 session 的字段名称，默认为 connect.sid 。
store: session 的存储方式，默认存放在内存中，也可以使用 redis，mongodb 等。express 生态中都有相应模块的支持。
secret: 通过设置的 secret 字符串，来计算 hash 值并放在 cookie 中，使产生的 signedCookie 防篡改。
cookie: 设置存放 session id 的 cookie 的相关选项，默认为 (default: { path: ‘/’, httpOnly: true, secure: false, maxAge: null })
genid: 产生一个新的 session_id 时，所使用的函数， 默认使用 uid2 这个 npm 包。
rolling: 每个请求都重新设置一个 cookie，默认为 false。
resave: 即使 session 没有被修改，也保存 session 值，默认为 true。
```

