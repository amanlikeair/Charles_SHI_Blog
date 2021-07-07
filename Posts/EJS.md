---
title: EJS
date: 2020-10-05 16:55:32
tags:
- Self Learning
- Note
- Web App
- EJS
categories:
- Developer
- Web App
---


# EJS相关笔记

***
### 初始化ejs
***

首先在项目根目录下安装ejs包  
`npm install ejs`  
之后编辑app.js主文件: express()实例化之后添加语段:  
```javascript
const app = express();
app.set("view engine", "ejs");
```
另外,项目根目录下新建名为views的文件夹里,所有的.ejs文件将放在这里, view engine将会默认查看它们  
`app.get("/", function(req, res) {....});`返回response时,  
添加语句:  
```javascript
res.render('list', {kindOfDay: day});
```
EJS会使用view engine来渲染名为list的.ejs文件(类似于原本的list.html)  
这样是在声明.ejs中的kindOfDay即是app.js中的day  
list.ejs被称为一个模板, 它根据app.js中传输的day来改变,决定向用户如何显示.  
此时代码为:  

```javascript
const express = require("express"); //引用npm包express
const bodyParser = require("body-parser"); //引用npm包body-parser
const app = express(); //实例化express
app.set("view engine", "ejs");
app.get("/", function(req, res) {
  var today = new Date();
  var currentDay = today.getDay();
  var day = "";
  if (currentDay === 6 || currentDay === 0) {
    day="Weekend";
  } else {
    day="Weekday";
  }
  res.render('list', {
    kindOfDay: day
  }
);
});
app.listen(3000, function() {
  console.log("Server is running on port 3000!   " + new Date().getHours() + ":" + new Date().getMinutes() + ":" + new Date().getSeconds());
});

```




***
### 在.ejs文件中运行js代码(逻辑语句)
***

需要将每一行contrl flow语句 (js代码) 包括在 <% ....%>中   
不同于声明参数对应关系时使用的的<%= .... %>  
原代码:  


```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>To Do List</title>
  </head>
  <body>

    if(kindOfDay === "Saturday" || kindOfDay === "Sunday"){
      <h1 style="color: purple"><%= kindOfDay%> ToDo List</h1>
    }else{
      <h1 style="color: blue"><%= kindOfDay%> ToDo List</h1>
    }

  </body>
</html>
```

需要修改为:

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
  <meta charset="utf-8">
  <title>To Do List</title>
</head>
<body>

  <% if(kindOfDay === "Saturday" || kindOfDay === "Sunday"){ %>
  <h1 style="color: purple"><%= kindOfDay%> ToDo List</h1>
  <% }else{ %>
  <h1 style="color: blue"><%= kindOfDay%> ToDo List</h1>
  <% } %>

</body>
</html>

```
