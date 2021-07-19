[< Home](https://amanlikeair.github.io/Charles_SHI_Blog/)


****
课程
Complete Web Development Bootcamp on Udemy
https://www.udemy.com/course/the-complete-web-development-bootcamp/
***
### 基本
一个web包括三个部分:
- HTML  CSS   JS
- 以及Back-end
- 使用node-js

常用工具/库:
- Bootstrap: 一个开源CSS框架
- jQuery: 一个JaveScript库
- Heroku: 一个免费服务器网站, 允许免费发布5个项目


### command line 命令行:

- `cd`   改变路径
- `ls`    列出
- `cd ~`    进入用户主目录
- `cd /`   进入根目录
- `cd ..`     返回前一个路径
- `mkdir`     新的路径
- `touch`     新建一个文件 touch hello.txt
- `start`     打开一个文件 start hello.txt  mac是open
- `start atom hello.txt`      用atom打开 mac是 open -a atom
- `hello.text`
- `rm hello.txt`    删除一个文件
- `rm *`       删除所有文件
- `rm -r imgs/`     删除一个目录



### 新建一个web app的步骤:
cd进入正确的目录下  
touch app.js 新建一个主js文件(名称不限)  
touch black.html red.html 新建几个html文件  
npm init 初始化npm(npm --verison查看npm版本)  
初始化信息:描述,入口:为app.js, git信息,作者.  
完成初始化后,目录下新增package.json文件,记录安装的所有npm的packages  
安装一些常用的npm包:  
- `npm install body-parser`     跟JSON文本的翻译有关,可在js中获取数据  
- `npm install express`         跟网络建立有关
- `npm install request`         跟数据请求有关  

(如还未设置git,可能有warn警告)  
安装完成后,目录下新增一个node_modules文件夹,和package-look.json文件  
开始编辑app.js:  
```JavaScript
const express = require("express");           //引用npm包express
const bodyParser = require("body-parser");    //引用npm包body-parser
const request = require("request");           //引用npm包request

const app = express();                        //实例化express

app.listen(3000,function(){
  console.log("Server is running on port 3000!");
});                                           //在网络3000口上收听,如成功则log显示信息
```
此时在命令行中node app.js启动本地服务器  
如果成功,console中应该显示"Server is running on port 3000!"  
在浏览器中输入localhost:3000/访问  
由于还没有声明GET / 的方法, 浏览器显示Cannot GET /  
ctrl+c 可以随时结束程序.  
每一次对代码修改之后, 都需要结束程序,再次启动.  
为了方便,可以安装Nodemon: 每一次修改代码之后能够自动重启.  
使用方法:使用nodemon app.js 代替 node app.js 启动程序
