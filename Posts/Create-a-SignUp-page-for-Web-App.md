[< Home](https://amanlikeair.github.io/Charles_SHI_Blog/)


# 新建signUp页面为例



### 回应GET / 请求, 指向html文件:  
前接上篇 笔记.txt 新建一个web app



编辑app.js主文件:

```JavaScript
const express = require("express");           //引用npm包express
const bodyParser = require("body-parser");    //引用npm包body-parser
const request = require("request");           //引用npm包request

const app = express();                        //实例化express

app.get("/",function(req,res){
  res.sendFile(__dirname + "/signup.html");
});                                 //当收听到GET/时, callback函数将发送文件singup.html作为回复
                                    //路径中加入__dirname防止错误

app.listen(3000,function(){
  console.log("Server is running on port 3000!!!!");
});                                           //在网络3000口上收听,如成功则log显示信息
```


此时若运行程序, 浏览器中.css和img无法加载  
**重要**:这是因为未将它们设置为静态  
需要添加语句, 并创建public文件夹:  


```javascript
const express = require("express");           //引用npm包express
const bodyParser = require("body-parser");    //引用npm包body-parser
const request = require("request");           //引用npm包request

const app = express();                        //实例化express

app.use(express.static("public"));            //将public文件夹设置为静态.
//需要手动建立一个项目根目录下的public文件夹,其中包括images和css两个子文件夹,放入所有css和图片


app.get("/",function(req,res){
  res.sendFile(__dirname + "/signup.html");
});

app.listen(3000,function(){
  console.log("Server is running on port 3000!!!!");
});                                           //在网络3000口上收听,如成功则log显示信息
```


修改之后, 可能需要将原来的一些href="...\styles.csss"路径修改  
**重要**: 但不需要在最前面加上 public/




###  回应POST / 请求, 返回表格中的数据:


需要使用bodyParser, 添加语句:  
```app.use(bodyParser.urlencoded({extended:ture}));```


代码为:
```javascript
const express = require("express");           //引用npm包express
const bodyParser = require("body-parser");    //引用npm包body-parser
const request = require("request");           //引用npm包request
const app = express();                        //实例化express

app.use(bodyParser.urlencoded({extended:ture}));    //使用bodyParser使得url内容可使用(POST方法时必需)

app.use(express.static("public"));            //将public文件夹设置为静态.
//需要手动建立一个项目根目录下的public文件夹,其中包括images和css两个子文件夹,放入所有css和图片

app.get("/",function(req,res){
  res.sendFile(__dirname + "/signup.html");
});

app.post("/", function(req, res) {
  var fisrtName = req.body.fName;             //获取html页面返回的数据(这是body-parser包的作用)
  var lastName = req.body.lName;
  var email = req.body.email;
  console.log(fisrtName, lastName, email);
});

app.listen(3000,function(){
  console.log("Server is running on port 3000!!!!");
});                                           //在网络3000口上收听,如成功则log显示信息
```



此时,勿要忘记修改html的部分:  
为html中元素声明 name:
```html
<input type="text" name="lName" class="form-control middle" placeholder="Last Name" required >
```
将表格的action声明为/ , 将method声明为POST  
```html
<form action="/"  class="form-signin" method="POST">
```
接下来,准备建立一个request, 按照API要求的格式将数据写入,再传给外部服务器:


```javascript
const express = require("express"); //引用npm包express
const bodyParser = require("body-parser"); //引用npm包body-parser
const request = require("request"); //引用npm包request
const https = require("https");     //引用https, 才能利用API传输

const app = express(); //实例化express

app.use(bodyParser.urlencoded({
  extended: true
})); //使用bodyParser使得url内容可使用(POST方法时必需)

app.use(express.static("public")); //将public文件夹设置为静态.
//需要手动建立一个项目根目录下的public文件夹,其中包括images和css两个子文件夹,放入所有css和图片

app.get("/", function(req, res) {
  res.sendFile(__dirname + "/signup.html");
});

app.post("/", function(req, res) {
  const firstName = req.body.fName;             //获取html页面返回的数据(这是body-parser包的作用)
  const lastName = req.body.lName;
  const email = req.body.email;
  console.log("用户已经输入: " +firstName, lastName, email);

  const data = {
    members:[                              //查询所用API的文档可知我们需要存入的内容的格式.
      {
        email_address: email,             //根据格式放入表格中获得的数据
        status:"subscribed",
        merge_fields:{
          FNAME:firstName,
          LNAME:lastName
        }
      }
    ]
  };
  const jsonData = JSON.stringify(data);    //将数据JSON化才能传给对方服务器

  //技术文档可知api地址为 "https://usX.api.mailchimp.com/3.0/lists" , 需要修改usX, 加入list id
  const url = "https://us2.api.mailchimp.com/3.0/lists/bf9926d032";
  //技术文档可知,还需要一个名为option的参数
  const options = {
    method:"POST",                                            //post方法
    auth: "yiding123:a997795bdfcc2d2be8dd9388511f51b8-us2"    //验证信息, 任意取的一个user name + api key
  }

  const request = https.request(url, options, function(response){     //准备好一个将要发送的request
    // response.on("data", function(data){
    //   console.log(JSON.parse(data));                                  //打印出待发送内容
    // })
    console.log("已经写入request准备发出: " + data.members[0].merge_fields.FNAME +" "+data.members[0].merge_fields.LNAME+" "+data.members[0].email_address);
  });

  request.write(jsonData);      //写入, 传给外部服务器
  request.end();

});
//MailChimp API KEY a997795bdfcc2d2be8dd9388511f51b8-us2
//Audience List id bf9926d032   mailchimp能够知道在什么地方加入你的订阅者
app.listen(3000, function() {
  console.log("Server is running on port 3000!   " + new Date().getHours() + ":" + new Date().getMinutes() + ":" + new Date().getSeconds());
}); //在网络3000口上收听,如成功则log显示信息


```




### sign up后, 跳转成功或失败页面:

我们需要判断request之后的response, 是否成功,即是否status code 为200, 而不是404或其他  
编辑代码,加入if判断:  


```javascript
const express = require("express"); //引用npm包express
const bodyParser = require("body-parser"); //引用npm包body-parser
const request = require("request"); //引用npm包request
const https = require("https");     //引用https, 才能利用API传输

const app = express(); //实例化express

app.use(bodyParser.urlencoded({
  extended: true
})); //使用bodyParser使得url内容可使用(POST方法时必需)

app.use(express.static("public")); //将public文件夹设置为静态.
//需要手动建立一个项目根目录下的public文件夹,其中包括images和css两个子文件夹,放入所有css和图片

app.get("/", function(req, res) {
  res.sendFile(__dirname + "/signup.html");
});

app.post("/", function(req, res) {
  const firstName = req.body.fName;             //获取html页面返回的数据(这是body-parser包的作用)
  const lastName = req.body.lName;
  const email = req.body.email;
  console.log("用户已经输入: " +firstName, lastName, email);

  const data = {
    members:[                              //查询所用API的文档可知我们需要存入的内容的格式.
      {
        email_address: email,             //根据格式放入表格中获得的数据
        status:"subscribed",
        merge_fields:{
          FNAME:firstName,
          LNAME:lastName
        }
      }
    ]
  };
  const jsonData = JSON.stringify(data);    //将数据JSON化才能传给对方服务器

  //技术文档可知api地址为 "https://usX.api.mailchimp.com/3.0/lists" , 需要修改usX, 加入list id
  const url = "https://us2.api.mailchimp.com/3.0/lists/bf9926d032";
  //技术文档可知,还需要一个名为option的参数
  const options = {
    method:"POST",                                            //post方法
    auth: "yiding123:!a997795bdfcc2d2be8dd9388511f51b8-us2"    //验证信息, 任意取的一个user name + api key
  }

  const request = https.request(url, options, function(response){     //准备好一个将要发送的request
    if(response.statusCode===200){                                   //判断是否发送成功
      res.sendFile(__dirname + "/success.html");                     //相应跳转至不同页面
    }else{
      res.sendFile(__dirname + "/failure.html");
    }
    console.log("已经写入request准备发出: " + data.members[0].merge_fields.FNAME +" "+data.members[0].merge_fields.LNAME+" "+data.members[0].email_address);
  });

  request.write(jsonData);      //写入, 传给外部服务器
  request.end();

});
//MailChimp API KEY a997795bdfcc2d2be8dd9388511f51b8-us2
//Audience List id bf9926d032   mailchimp能够知道在什么地方加入你的订阅者
app.listen(3000, function() {
  console.log("Server is running on port 3000!   " + new Date().getHours() + ":" + new Date().getMinutes() + ":" + new Date().getSeconds());
}); //在网络3000口上收听,如成功则log显示信息
```





### 若sign up失败跳转至failure页面后, 设置一个按钮跳转回index.html

应该在failure.html设置按钮, 并设置action和method:  
```html
<form action="/failure" method="post">
  <button class="btn btn-warning btn-lg"  type="submit" name="button">Try agian!</button>
</form>
```


并在app.js主文件中, `app.post("/", function(req, res) {......});`语段之后,  
声明新的:  
```JavaScript
app.post("/failure", function(req,res){
  res.redirect("/");
});
```
使重定向到"/", (即重新 GET / )  
app.js完整代码如下:  

```javascript
const express = require("express"); //引用npm包express
const bodyParser = require("body-parser"); //引用npm包body-parser
const request = require("request"); //引用npm包request
const https = require("https");     //引用https, 才能利用API传输

const app = express(); //实例化express

app.use(bodyParser.urlencoded({
  extended: true
})); //使用bodyParser使得url内容可使用(POST方法时必需)

app.use(express.static("public")); //将public文件夹设置为静态.
//需要手动建立一个项目根目录下的public文件夹,其中包括images和css两个子文件夹,放入所有css和图片

app.get("/", function(req, res) {
  res.sendFile(__dirname + "/signup.html");
});

app.post("/", function(req, res) {
  const firstName = req.body.fName;             //获取html页面返回的数据(这是body-parser包的作用)
  const lastName = req.body.lName;
  const email = req.body.email;
  console.log("用户已经输入: " +firstName, lastName, email);

  const data = {
    members:[                              //查询所用API的文档可知我们需要存入的内容的格式.
      {
        email_address: email,             //根据格式放入表格中获得的数据
        status:"subscribed",
        merge_fields:{
          FNAME:firstName,
          LNAME:lastName
        }
      }
    ]
  };
  const jsonData = JSON.stringify(data);    //将数据JSON化才能传给对方服务器

  //技术文档可知api地址为 "https://usX.api.mailchimp.com/3.0/lists" , 需要修改usX, 加入list id
  const url = "https://us2.api.mailchimp.com/3.0/lists/bf9926d032";
  //技术文档可知,还需要一个名为option的参数
  const options = {
    method:"POST",                                            //post方法
    auth: "yiding123:!a997795bdfcc2d2be8dd9388511f51b8-us2"    //验证信息, 任意取的一个user name + api key
  }

  const request = https.request(url, options, function(response){     //准备好一个将要发送的request
    if(response.statusCode===200){
      res.sendFile(__dirname + "/success.html");
      console.log("成功,跳转至success页面");
    }else{
      res.sendFile(__dirname + "/failure.html");
      console.log("失败,跳转至failure页面");
    }
  });

  request.write(jsonData);      //写入, 传给外部服务器
  request.end();

});
//MailChimp API KEY a997795bdfcc2d2be8dd9388511f51b8-us2
//Audience List id bf9926d032   mailchimp能够知道在什么地方加入你的订阅者

app.post("/failure", function(req,res){
  res.redirect("/");
});

app.listen(3000, function() {
  console.log("Server is running on port 3000!   " + new Date().getHours() + ":" + new Date().getMinutes() + ":" + new Date().getSeconds());
}); //在网络3000口上收听,如成功则log显示信息
```
