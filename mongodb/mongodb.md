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
 show dbs //查看所有的数据库
 ```
