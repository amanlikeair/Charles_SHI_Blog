---
title: REST and API
date: 2020-10-05 17:01:35
tags:
- Self Learning
- Note
- Web App
- REST
categories:
- Developer
- Web App
---

# REST笔记
代码实践在文件中Wiki-API文件夹中.
#### HTTP Request verbs
- GET
- POST
- PUT & PATCH
- DELETE

对应于数据库中的READ CREATE UPDATE DELETE

PUT和PATCH都是是更新, PUT是请求一整个新的数据, PATCH是只请求部分数据(换单车和换轮胎的区别)


# 创建一个REST route (wiki风格)

#### 设计: 本案例中对于不同路径的操作和结果
| HTTP verbs | /articles | /articles/jack |
| :-----| :---- | :---- |
| GET | Fertches all the article | Fetches the article on jack |
| POST | Create one new article | - |
| PUT | - | Updates the article on jack |
| PATCH | - | Updates the article on jack |
| DELETE | Deletes all the articles | Deletes the article on jack |

#### 步骤

下载一个可视化的MongoDB管理软件 ROBO 3T

shell中 代开mongoDB `mongod`

ROBO 3T中create一个新的连接

localhost 默认端口27017

创建一个新的database wikiDB

创建一个新的collections articles

articles上点击 insert document 键入数据

```JSON
{
    title: "REST"
    content: "REST is today class."
}
```
以及粘贴:
```JSON
{
    "_id" : ObjectId("5c139771d79ac8eac11e754a"),
    "title" : "API",
    "content" : "API stands for Application Programming Interface. It is a set of subroutine definitions, communication protocols, and tools for building software. In general terms, it is a set of clearly defined methods of communication among various components. A good API makes it easier to develop a computer program by providing all the building blocks, which are then put together by the programmer."
}


{
    "_id" : ObjectId("5c1398aad79ac8eac11e7561"),
    "title" : "Bootstrap",
    "content" : "This is a framework developed by Twitter that contains pre-made front-end templates for web design"
}


{
    "_id" : ObjectId("5c1398ecd79ac8eac11e7567"),
    "title" : "DOM",
    "content" : "The Document Object Model is like an API for interacting with our HTML"
}
```
### 初始化项目

进入新建立的项目目录下:

`npm init -y`

`npm i body-parser mongoose ejs express`

`touch app.js`

### 编辑主文件app.js
粘贴代码:
```JavaScript
//jshint esversion:6

const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require('mongoose');

const app = express();

app.set('view engine', 'ejs');

app.use(bodyParser.urlencoded({
  extended: true
}));
app.use(express.static("public"));

//TODO

app.listen(3000, function() {
  console.log("Server started on port 3000");
});

```

添加语句,连接数据库:

```javascript
mongoose.connect("mongodb://localhost:27017/wikiDB",{ useUnifiedTopology: true, useNewUrlParser: true });
```

建立article的schema, 新建一个aricles表格(已经建立):

```javascript
const articleSchema = {
  title: String,
  content: String
};

const Aricle = mongoose.model("Article", articleSchema);
```

最终代码为:
```JavaScript
//jshint esversion:6

const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require('mongoose');

const app = express();

app.set('view engine', 'ejs');

app.use(bodyParser.urlencoded({
  extended: true
}));
app.use(express.static("public"));

mongoose.connect("mongodb://localhost:27017/wikiDB",{ useUnifiedTopology: true, useNewUrlParser: true });

const articleSchema = {
  title: String,
  content: String
};

const Aricle = mongoose.model("Article", articleSchema);

app.listen(3000, function() {
  console.log("Server started on port 3000");
});

```

### 根据设计,逐个实现

##### GET /article:
```JavaScript
app.get("/articles", function(req,res){
  Article.find(function(err, foundArticles){
    if(!err){
      res.send(foundArticles);
    }else{
      res.send(err);
    }
  });
});
```

##### POST /article:
使用工具 Postman用于测试POST方法

软件内, 左侧选择collections, 右侧改为POST, 输入/localhost:3000/articles

下方选择body, 选择x-www-form-unlencoded

输入title和content 以及他们的内容

```JavaScript
app.post("/articles",function(req,res){
  const newArticle = new Article({
    title: req.body.title,
    content: req.body.content
  });
  newArticle.save(function(err){
    if(!err){
      res.send("Successfully add a new article.");
    }else{
      res.send(err);
    }
  });
});
```

##### DELETE /article:
```JavaScript
app.delete("/articles",function(req,res){
  Article.deleteMany(function(err){
    if(!err){
      res.send("Successfully delete all articles.");
    }else{
      res.send(err);
    }
  });
});
```
#### 简洁方法: Chained Route Using Express

Express有一种简单方法重新排列相同路径下的get post delete:

```JavaScript
app.route('/book')
  .get(function (req, res) { ...
  })
  .post(function (req, res) { ...
  })
  .put(function (req, res) { ...
  })
```

所以, 上面三段可以合并为:
```JavaScript
app.route('/book').get(...).post(...).delete(...);
```

即:
```JavaScript
app.route('/articles')

.get(function(req,res){
  Article.find(function(err, foundArticles){
    if(!err){
      res.send(foundArticles);
    }else{
      res.send(err);
    }
  });
})

.post(function(req,res){
  const newArticle = new Article({
    title: req.body.title,
    content: req.body.content
  });
  newArticle.save(function(err){
    if(!err){
      res.send("Successfully add a new article.");
    }else{
      res.send(err);
    }
  });
})

.delete(function(req,res){
  Article.deleteMany(function(err){
    if(!err){
      res.send("Successfully delete all articles.");
    }else{
      res.send(err);
    }
  });
});
```

##### 对于某个特定article的操作:

##### GET /article/某篇article:
使用chained route
```JavaScript
app.route("/articles/:articleTitle").get().put().patch().delete();
```
```JavaScript
.get(function(req,res){
  Article.findOne({title: req.params.articleTitle}, function(err,foundArticle){
    if(foundArticle){
      res.send(foundArticle);
    }else{
      res.send("No article matching this title was found.")
    }
  });
})
```
如果是路径中有空格,则用%20代替. 如/articles/Jack%20James


##### PUT /article/某篇article:
```JavaScript
.put(function(req,res){
  Article.update(
    {title: req.params.articleTitle},
    {title: req.body.title, content: req.body.content}, //可以同时更改title和content.但改title之后路径就不对了
    {overwrite: true},
    function(err){
      if(!err){
        res.send("Successfully update article.");
      }else{
        res.send(err);
      }
    }
  );
})
```
如果req时只提供title信息,而无content信息,查看数据库可见将没有content信息. 这是因为PUT方法是整体更新

##### PATCH /article/某篇article:
```JavaScript
.patch(function(req,res){
  Article.update(
    {title: req.params.articleTitle},
    {$set: req.body},  //将body设置为req里的内容,这样无论是哪个信息被改变,都能被传递()
    function(err){
      if(!err){
        res.send("Successfully update article.");
      }else{
        res.send(err);
      }
    }
  );
})
```

##### DELETE /article/某篇article:
```JavaScript
.delete(function(req,res){
  Article.deleteOne(
    {title: req.params.articleTitle},
    function(err){
      if(!err){
        res.send("Successfully delete the matching article.");
      }else{
        res.send(err);
      }
    }
  );
});   //不要忘了最后的分号,
//为app.route("/articles/:articleTitle").get().put().patch().delete();结尾
```
