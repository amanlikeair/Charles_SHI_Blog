[< Home](https://amanlikeair.github.io/Charles_SHI_Blog/)


# Authentication & Sercurity笔记


## 多层安全:
- level 1: Email & Password
- level 2: Encryption
- level 3: Hash
- level 4: Salting and Hashing
- level 5: Cookies & Sessions
- level 6: OAuth 第三方网站授权( ....未完成)


## level 1: Email & Password
()依靠数据库信息)

项目目录下:
`npm i mongoose`

然后,主文件中载入mongoose, 两段代码:
```javascript
const mongoose = require('mongoose');
```
```javascript
mongoose.connect("mongodb://localhost:27017/fruitsDB", { useNewUrlParser: true, useUnifiedTopology: true});
```

之后写get /register:
```javascript
app.post("/register",function(req,res){
  const newUser = new User({
    email: req.body.username,
    password: req.body.password
  });
  newUser.save(function(err){
    if(!err){
      res.render("secrets");
    }else{
      res.send(err);
    }
  });
});
```

之后写POST /register:
```javascript
app.post("/register",function(req,res){
  const newUser = new User({
    email: req.body.username,
    password: req.body.password
  });
  newUser.save(function(err){
    if(!err){
      res.render("secrets");
    }else{
      res.send(err);
    }
  });
});
```

和POST /login
```javascript
app.post("/login",function(req,res){
  const username = req.body.username;
  const password = req.body.password;
  User.findOne({email: username}, function(err,foundUser){
    if(err){
      console.log(err);
    }else{
      if(foundUser){
        if (foundUser.password === password){
          res.render("secrets");
        }
      }
    }
  })
});
```
<p style="color:red;">此时, 密码是以明文普通文本保存. 危险性大</p>



## level 2: Encryption

各种加密方式, 这里选用mongoose-encryption包:

安装: `npm i mongoose-encryption`

主文件中:
```JavaScript
const encrypt = require("mongoose-encryption");
```

然后,根据要求改写schema格式:
```JavaScript
const userSchema = new mongoose.Schema({
  email: String,
  password: String
});
```

随意建立一个字符串,作为密码种子:
```JavaScript
const secret = "Thisisourlittlesecret.";
```

然后,加密密码(使用种子):
encryption是mongoose的一个插件
```javascript
userSchema.plugin( encrypt, {secret: secret, encryptedFields:["password"]} );
```

测试运行, 发现新用户注册后数据库内数据的密码部分不再是普通文本, 而变成了二进制串.

<p style="color:red;">但这种方式,加密种子仍然能被他人获知</p>

我们可以采用enviroment variables来解决.

安装包: `npm i dotenv`

在主文件最上方插入代码:
```JavaScript
require('dotenv').config(); //必须加在最上面
```
接着,项目根目录下创建文件 .env: `touch .env`

将主文件中的密码种子剪切粘贴到.env文件:
```JavaScript
const secret = "Thisisourlittlesecret.";
```
===>
```JavaScript
SECRET = Thisisourlittlesecret.
```

其他的一些重要内容之后也可以保存在这, 例如 API_KEY. 建议保持大写和_的格式

主文件中若想调用, 则可使用格式为:
```JavaScript
process.env.API_KEY;
```

所以, 主文件中代码变为:
```JavaScript
userSchema.plugin( encrypt, {secret: process.env.SECRET, encryptedFields:["password"]} );
```

接下来, 采用设置gitignore的方法, 使部分文件在git push的时候不上传,从而保密.

下面的连接是官方文档, 告诉一些类型的项目那些内容可以ignore:

[gitignore 连接](https://github.com/github/gitignore)

我们是nodeJS项目, 所以选择Node.采用设置gitignore

[Node.gitignore](https://github.com/github/gitignore/blob/master/Node.gitignore)

有一些文件通常需要ignore:

- node_modules/

- jspm_packages/

- .env

在项目根目录下新建.gitignore文件 `touch .gitignore`

在里面直接粘贴node.gitignore里面的文字,保存

可以看到, atom里, 左侧文件管理中的.env文件颜色变浅, 说明隐藏成功

之后可以push到github上. 另外,因为github会保留修改记录, 之前普通文本所写的信息依然会被看到,

因此需要每一次建立项目时就创建environment variables.

另外, 一些网站托管服务器(如Heroku)有自己的方法储存envi vars, 有时有可视化界面供设置.


目前完整代码如下:
```JavaScript
//jshint esversion:6
require('dotenv').config(); //必须加在最上面
const express = require("express");           //引用npm包express
const bodyParser = require("body-parser");    //引用npm包body-parser
const ejs = require("ejs");           //引用npm包request
const mongoose = require('mongoose');
const encrypt = require("mongoose-encryption");

const app = express();

app.use(express.static("public"));
app.set('view engine', 'ejs');
app.use(bodyParser.urlencoded({
  extended: true
}));

mongoose.connect("mongodb://localhost:27017/userDB", { useNewUrlParser: true, useUnifiedTopology: true});

const userSchema = new mongoose.Schema({
  email: String,
  password: String
});


userSchema.plugin( encrypt, {secret: process.env.SECRET, encryptedFields:["password"]} );

const User = new mongoose.model("User", userSchema);

app.get("/",function(req,res){
  res.render("home");
});

app.get("/login",function(req,res){
  res.render("login");
});

app.get("/register",function(req,res){
  res.render("register");
});

app.post("/register",function(req,res){
  const newUser = new User({
    email: req.body.username,
    password: req.body.password
  });
  newUser.save(function(err){
    if(!err){
      res.render("secrets");
    }else{
      res.send(err);
    }
  });
});

app.post("/login",function(req,res){
  const username = req.body.username;
  const password = req.body.password;
  User.findOne({email: username}, function(err,foundUser){
    if(err){
      console.log(err);
    }else{
      if(foundUser){
        if (foundUser.password === password){
          res.render("secrets");
        }
      }
    }
  })
});

app.listen(3000, function() {
  console.log("Server started on port 3000");
});

```



### level 3: Hash

归根到底, 密码种子总有可能被得到而解密

可以使用Hash方法:

添加包 `npm i md5`

主文件内, 移除加密语段

```
userSchema.plugin( encrypt, {secret: process.env.SECRET, encryptedFields:["password"]} );
```

添加引用md5:
```javascript
const md5 = require("md5");
```

修改 POST /register 方法,使得存入数据库的密码是一个hash数:
```javascript
app.post("/register",function(req,res){
  const newUser = new User({
    email: req.body.username,
    password: md5(req.body.password)    //hash一下
  });
  newUser.save(function(err){
    if(!err){
      res.render("secrets");
    }else{
      res.send(err);
    }
  });
});
```

以及 POST /login 方法,使得以一个hash数与数据库中的进行对比:
```javascript
app.post("/login",function(req,res){
  const username = req.body.username;
  const password = md5(req.body.password);    //hash一下
  User.findOne({email: username}, function(err,foundUser){
    if(err){
      console.log(err);
    }else{
      if(foundUser){
        if (foundUser.password === password){
          res.render("secrets");
        }
      }
    }
  })
});
```

## level 4 Salting and Hashing

需要改进, hash需要加盐

项目目录下, 添加包: `npm i bcrypt`

主文件修改代码:
```JavaScript
// const md5 = require("md5");        //改用bcrypt
const bcrypt = require("bcrypt");

const saltRounds = 10;
```

加盐的round数更大更安全, 只是消耗的时间更长

修改 POST /register 方法,使得存入数据库的密码是一个加盐的hash数:
```javascript
app.post("/register",function(req,res){
  const newUser = new User({
    email: req.body.username,
    password: md5(req.body.password)    //hash一下
  });
  newUser.save(function(err){
    if(!err){
      res.render("secrets");
    }else{
      res.send(err);
    }
  });
});
```

## level 5: Cookies & Sessions

cookies中常包括一些用户信息:

例如用户浏览并添加在购物车中的信息, GET /方法时候会连同cookies一同发给服务器,

服务器会根据cookie中的信息回应用户, 包括意向物品的信息.

Session是client和server交互的一段时间.

添加passport包来使用cookie: 以及一些其他的包:

```
npm i passport
npm i passport-local
npm i passport-local-mongoose
npm i express-session
```
注意包的准确的拼写

主文件中, 移除代码:

```JavaScript
// const bcrypt = require("bcrypt");   //改用passport
// const saltRounds = 10; //改用passport
```

添加:
```JavaScript
const session = require('express-session');
const passport = require('passport');
const passporLocalMongoose = require('passport-local-mongoose');
```

设置session:
```JavaScript
app.use(session({
  secret:  "Our little secret",
  resave: false,
  saveUninitialized: false
}));
```

设置passport:
```JavaScript
app.use(passport, initialized());
app.use(passport.session());
```

对UserSchema处理, 加盐hash储存在数据库中:
```JavaScript
userSchema.plugin(passporLocalMongoose);
```

passport处理:
```JavaScript
passport.use(User.createStrategy());

passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());
```

运行app.js, 系统报错`(node:13412) DeprecationWarning: collection.ensureIndex is deprecated. Use createIndexes instead.`

google可知, 需要添加语句:
```JavaScript
mongoose.set("useCreateIndex", true);
```

接下来修改post /register 方法:
```JavaScript
app.post("/register",function(req,res){
  User.register({username: req.body.username}, req.body.password, function(err, user){
    if(err){
      console.log(err);
      res.redirect("/register");
    }else{
      passport.authenticate("local")(req, res, function(){
        res.redirect("/secrets");
      });
    }
  });
});
```

添加get /secret 方法:
```JavaScript
app.get("/secrets",function(req,res){
  if(req.isAuthenticated()){
    res.render("secrets");
  }else{
    res.render("/login");
  }
});
```

这时候运行, register一个新用户, 在数据库内查看,将会发现新的一行多了一个salt和hash的参数和数值.

此时, 回到主页之后,地址栏中可以直接访问`http://localhost:3000/secrets`,这就是因为cookies记住了我们网站的已登陆状态.

如果退出浏览器(ctrl+q), 再进入,再次访问/secrets的话,则会转到login页面.


这时候, 修改post /login 方法:
```JavaScript
app.post("/login",function(req,res){
  const user = new User({
    username: req.body.username,
    password: req.body.password
  });
  req.login(user, function(err){
    if(err){
      console.log(err);
    }else{
      passport.authenticate("local")(req, res, function(){
        res.redirect("/secrets");
      });
    }
  });
});
```

添加get /logout 方法:
```JavaScript
app.get("/logout",function(req,res){
  req.logout();
  res.redirect("/");
});
```

最后的完整代码为:
```JavaScript
//jshint esversion:6
require('dotenv').config(); //必须加在最上面
const express = require("express");           //引用npm包express
const bodyParser = require("body-parser");    //引用npm包body-parser
const ejs = require("ejs");           //引用npm包request
const mongoose = require('mongoose');
// const encrypt = require("mongoose-encryption");    //改用hash方法加密了
// const md5 = require("md5");        //改用bcrypt
// const bcrypt = require("bcrypt");   //改用passport
// const saltRounds = 10; //改用passport
const session = require('express-session');
const passport = require('passport');
const passporLocalMongoose = require('passport-local-mongoose');

const app = express();

app.use(express.static("public"));
app.set('view engine', 'ejs');
app.use(bodyParser.urlencoded({
  extended: true
}));

app.use(session({
  secret:  "Our little secret",
  resave: false,
  saveUninitialized: false
}));

app.use(passport.initialize());
app.use(passport.session());

mongoose.connect("mongodb://localhost:27017/userDB", { useNewUrlParser: true, useUnifiedTopology: true});
mongoose.set("useCreateIndex", true);

const userSchema = new mongoose.Schema({
  email: String,
  password: String
});

userSchema.plugin(passporLocalMongoose);

const User = new mongoose.model("User", userSchema);

passport.use(User.createStrategy());

passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());

app.get("/",function(req,res){
  res.render("home");
});

app.get("/login",function(req,res){
  res.render("login");
});

app.get("/register",function(req,res){
  res.render("register");
});

app.get("/secrets",function(req,res){
  if(req.isAuthenticated()){
    res.render("secrets");
  }else{
    res.redirect("/login");
  }
});

app.get("/logout",function(req,res){
  req.logout();
  res.redirect("/");
});

app.post("/register",function(req,res){
  User.register({username: req.body.username}, req.body.password, function(err, user){
    if(err){
      console.log(err);
      res.redirect("/register");
    }else{
      passport.authenticate("local")(req, res, function(){
        res.redirect("/secrets");
      });
    }
  });
});

app.post("/login",function(req,res){
  const user = new User({
    username: req.body.username,
    password: req.body.password
  });
  req.login(user, function(err){
    if(err){
      console.log(err);
    }else{
      passport.authenticate("local")(req, res, function(){
        res.redirect("/secrets");
      });
    }
  });
});

app.listen(3000, function() {
  console.log("Server started on port 3000");
});

```


## level 6: OAuth 第三方网站授权
