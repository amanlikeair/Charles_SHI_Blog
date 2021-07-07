---
title: Publish app to Web
date: 2020-10-05 16:49:31
banner_img: 
tags:
- Self Learning
- Note
- Web App
- Heroku
categories:
- Developer
- Web App
---

# app发布至网络_Heroku为例


### 入门: 在网络上发布app, 利用heroku网站

官方网站安装heroku  
```
$ heroku login
```
首先, app.js中的语句修改为:  
```javascript
app.listen(process.env.PORT, function() {...});
//根据托管服务器的环境确定端口,或在3000本地收听
```

然后,根据Heroku网站要求,需要在项目根目录下新建一个Procfile的文件, 无后缀名  
文件中输入`web: node app.js`, 告诉网站以何种方式运行主文件  
完成之后, 需要初始化heroku:  `heroku create`

之后需要初始化git:
```
git init
git add .
git commit -m "first commit"
```

之后需要push到heroku  
```
git push heroky master
```
成功后,结果中将出现网址,即可访问
