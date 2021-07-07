---
title: MongoDB
date: 2020-10-05 16:59:14
tags:
- Self Learning
- Note
- Web App
- MongoDB
- Database
categories:
- Developer
- Web App
---

# MongoDB笔记

* * *

2020-Sep-25  
Complete Web Development Bootcamp on Udemy  
<https://www.udemy.com/course/the-complete-web-development-bootcamp/>

* * *

### 安装

MongoDB网站下载镜像: [MongoDB网站](https://www.mongodb.com/try/download/community)

安装在默认位置: C:\\Program Files\\MongoDB\\Server\\4.4\\data\\

安装完成后, 在C盘下新建名为data的文件夹, data下新建名为db的文件夹

命令行中, `cd ~`

然后新建 .bash_profile文件 `toch .bash_profile`

用vim打开, `vim .bash_profile`

`i` 进入编辑模式, 粘贴以下代码:

`/4.X`修改为实际版本号

    alias mongod="/c/Program\ files/MongoDB/Server/4.X/bin/mongod.exe"
    alias mongo="/c/Program\ Files/MongoDB/Server/4.X/bin/mongo.exe"

`esc`键, 之后`:wq!`保存

重启命令行, `mongo --version`检查是否安装成功.

# 基本使用操作

### 启动

`mongod` 启动mongo

将显示信息:

    Waiting for connections","attr":{"port":27017

即此时已经成功启动,等待连接, 端口是27017

持续运行保持命令行不关闭

### 启动Mongo shell

`mongo` 启动mongo shell

我们可以在>之后键入指令

### mongo shell 中操作

`show dbs` 显示已有databases

`use shopDB` 新建/切换到某个database. 刚新建之后利用`show dbs`查询不显示, 因为刚新建没有数据

`db` 显示当前所在db

`show collections` 显示db下所有table(或collections)

`db.dropDatabase()` 删除当前数据库

##### 增:

新建table,和第一行数据:

`db.products.insertOne({ _id: 1, name: "Pen", price: 1.20})`

##### 查:

查看table中数据:

`db.products.find()`

根据条件,查看table中数据:

`db.products.find({name: "Pencil"})`

`db.products.find({price: {$gt: 1.0}})`  价格大于1的

或者指定展示哪些列, {\_id: 1}或{\_id: 0}表示是否展示这一列:  
`db.products.find({_id: 1}, {name: 1})`

其他query符:

-   `$eq` Matches values that are equal to a specified value.  
-   `$gt`	Matches values that are greater than a specified value.  
-   `$gte`	Matches values that are greater than or equal to a specified value.  
-   `$in`	Matches any of the values specified in an array.  
-   `$lt`	Matches values that are less than a specified value.  
-   `$lte`	Matches values that are less than or equal to a specified value.  
-   `$ne`	Matches all values that are not equal to a specified value.  
-   `$nin`	Matches none of the values specified in an array.  

##### 改:

为\_id为2的行添加自己的一列, 名为stock, 数值32 (之前并没有这一列),  
(操作后只有这一行数据有这一列信息.这是mongoDB不同于SQL的).
`db.products.updateOne({_id:2}, {$set: {stock: 32}})`

##### 删:

db.products.delteOne({id: 2})

### MongoDB中数据的特殊结构

可以这样定义一行数据:  
一个产品,包括对他的两个顾客评价:

```sql
db.products.insert(
  {
    _id: 3,
    name: "ruby",
    price: 2.3,
    stock: 22,
    review: [
      {
        author: "Jack",
        rating: 4,
        review: "Good!"
      },
      {
        author: "Tom",
        rating: 5,
        review: "Nice one!"
      }
    ]
  }
)
```


# MondDb与Node Js协作
连接MondDb与Node Js的两种方式:
- MongoDB native driver(复杂,不常用)
- Mongoose(一种ODM Object Document Mapper,代码简洁,常用)

### ~~MongoDB native driver:~~
~~代码复杂...~~

### Mongoose:

##### 使用案例

首先项目根目录下`npm install mongoose`安装包

app.js主文件如下:

>将会新建(若有则修改)一个数据库fruitsDB. 数据库中创建一个叫fuits的table. Table中新增一行name为Apple的数据.

```JavaScript
//引用mongoose
const mongoose = require('mongoose');
//连接在本地已经启动的数据库
mongoose.connect("mongodb://localhost:27017/fruitsDB", { useNewUrlParser: true, useUnifiedTopology: true});
//后面两个为true的参数是根据Mongo所报warn被要求添加的.未来版本mongo可能会消失

//准备一个数据结构模板
const fruitSchema = new mongoose.Schema ({
  name: String,
  rating: Number,
  review: String
});

//在数据库中以"Fruit"建立table, mongo会根据"fruit"智能地建立名为fruits(复数形式)的table
const Fruit = mongoose.model("Fruit", fruitSchema);

//一个准备写入table的一行新数据
const fruit = new Fruit({
  name: "Apple",
  rating: 7,
  review: "Pretty solid as a fruit."
});

//写入这一行
fruit.save();
```

若同时多个数据需要插入, 可使用语句:  
callback函数用来报错
```JavaScript
Fruit.insertMany([kiwi, orange, apple], function(err){
  if (err){
    console.log(err);
  }
    console.log("Successfully save all");
})
```

可添加一下语句:  
打印出所有fruits表格内的数据行
```JavaScript
Fruit.find(function(err, fruits){
  if (err){
    console.log(err);
  }else{
    console.log(fruits);
  }
});

```

或者这样, 只打印出数据行的名字:
```JavaScript
Fruit.find(function(err, fruits){
  if (err){
    console.log(err);
  }else{
    fruits.forEach(function(fruit){
      console.log(fruit.name)
    });
  }
});
```

最后不要忘了关闭与数据库的连接, 否则将一直保持连接.
```JavaScript
mongoose.connection.close();
```

完整代码如下:

```JavaScript
//引用mongoose
const mongoose = require('mongoose');
//连接在本地已经启动的数据库
mongoose.connect("mongodb://localhost:27017/fruitsDB", { useNewUrlParser: true, useUnifiedTopology: true});
//后面两个为true的参数是根据Mongo所报warn被要求添加的.未来版本mongo可能会消失

//准备一个数据结构模板
const fruitSchema = new mongoose.Schema ({
  name: String,
  rating: Number,
  review: String
});

//在数据库中以"Fruit"建立table, mongo会根据"fruit"智能地建立名为fruits(复数形式)的table
const Fruit = mongoose.model("Fruit", fruitSchema);

//一个准备写入table的一行新数据
const apple = new Fruit({
  name: "Apple",
  rating: 7,
  review: "Pretty solid as a fruit."
});

//另一行
const kiwi = new Fruit({
  name: "kiwi",
  rating: 2,
  review: "Not good."
});

//写入多行
Fruit.insertMany([kiwi, apple], function(err){
  if (err){
    console.log(err);
  }else{
    console.log("Successfully save all");
  }
})

//打印出所有fruits表格内的数据行(的name)
Fruit.find(function(err, fruits){
  if (err){
    console.log(err);
  }else{
    fruits.forEach(function(fruit){
      console.log(fruit.name)
      mongoose.connection.close(); //关闭与数据库的连接
    });
  }
});
```

##### 进一步: Data validation数据验证
原代码中:
```javascript
const fruitSchema = new mongoose.Schema ({
  name: String,
  rating: Number,
  review: String
});
```

可以对某个数据的格式进行规定:

规定rating为numer类型, 最小1最大10
```javascript
const fruitSchema = new mongoose.Schema ({
  name: String,
  rating: {
    type: Number,
    min: 1,
    max: 10
  },
  review: String
});
```
