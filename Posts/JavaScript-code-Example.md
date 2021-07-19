[< Home](https://amanlikeair.github.io/Charles_SHI_Blog/)




# JS代码案例

* * *

2020-Sep-21  
Complete Web Development Bootcamp on Udemy  
<https://www.udemy.com/course/the-complete-web-development-bootcamp/>
JavaScript example sentences  

* * *

选择h1字体的内容 改变颜色:

```javascript
document.querySelector("h1").style.color = "yellow"
```

选择a锚 获得链接中的地址:

```javascript
document.querySelector("a").getAttribute("href");
```

选择a锚 设置链接中的地址 设置为google:

```javascript
document.querySelector("a").setAttribute("href", "https://google.com");
类似的:
document.querySelectorAll("img")[1].setAttribute("src", randomImageSource2);
```

选择h1字体的内容 将它html中的文本段改为Good bye. 即: `<h1>blue sky</h1>` 变为`<h1>Good bye</h1>`

```javascript
document.querySelector("h1").innerHTML = "Good bye";
```

选择h1字体的内容 将它html中的文本段改为<em>Good bye</em>. 即: `<h1>blue sky</h1>` 变为`<h1><em>Good bye</em></h1>`

```javascript
document.querySelector("h1").innerHTML = "<em>Good bye<em>";
```

选择h1字体的内容 返回文本内容:

```javascript
document.querySelector("h1").textContent;
```

取随机数:

```javascript
Math.random();
```

取整数:

```javascript
Math.floor();
```

if else else 语句:

```javascript
if (randomNumber1 > randomNumber2) {
  ;
} else if (randomNumber2 > randomNumber1) {
  ;
} else {
  ;
}
```

为一个button设置一个listener, click时触发handleClick. 第一句时handleClick后不需要(),否则将会直接被触发,而不是click后

```javascript
document.querySelector("button").addEventListener("click", handleClick);

function handleClick() {
  alert("hello");
}
```

如何为所有button添加Listener??  
获知html中名为.drum类的按钮的元素的数量

```javascript
var numberOfButton = document.querySelectorAll(".drum").length;
```

或让selector寻找button

```javascript
var numberOfButton = document.querySelectorAll("button").length;
```

然后,为所有的这些元素设置Listener, click将触发 handleClick函数  

```javascript
for (var i = 0; i &lt; numberOfButton; i++) {
  document.querySelectorAll(".drum")[i].addEventListener("click", handleClick);
}
```

讲一个函数作为另一个函数的参数:

```javascript
function multiply(num1, num2) {
  return num1 \* num2;
}

function divide(num1, num2) {
  return num1 / num2;
}

function calculator(num1, num2, operator) {
  return operator(num1, num2);
}
```

JS里播放声音:

```javascript
var audio = new Audio('crash.mp3');
audio.play();
```

JS中的多媒体:

```javascript
HTMLAudioElement
HTMLAudioElement.play()
HTMLAudioElement.autoplay()
HTMLAudioElement.pause()
```

类与对象:  
JS 类的构造函数
```javascript
function Boy(name, age, language) {
  this.name = name;
  this.age = age;
  this.language = language;
}
```

类的实例:
```javascript
var boy1 = new Boy("Tommy", 18, ["French", "English"]);
```

switch语句:   
判断键盘什么键被按下
```javascript
var buttonInnerHTML = this.innerHTML;
switch (buttonInnerHTML) {
  case "w":
    var audio1 = new Audio('1.mp3');
    audio1.play();
  case "s":
    var audio2 = new Audio('2.mp3');
    audio2.play();
  case "d":
    var audio3 = new Audio('3.mp3');
    audio3.play();
    break;
  default:
}
```

为整个网站添加键盘的EventListener:  
此处`function(){...}` 是一个anonymous function 匿名函数  
```javascript
document.addEventListener("keypress", function(){alert("key is pressed!")});
```

回调函数 callback 案例:
```javascript
function doSomething(msg, callback){
    alert(msg);
    if(typeof callback == "function")
    callback();
 }
doSomething("回调函数", function(){
    alert("匿名函数实现回调!");
 });
 ```
案例:  
键盘按下时, 报告事件详情  
event中将包括很多信息, event.key将返回键名的字符,如"w"  
```javascript
document.addEventListener("keypress", function(event){console.log(event)});
```
jQuery 等同于$
```javascript
jQuery("h1");
$("h1");
```

等待jQuery加载成功, 之后执行.js文件中的语句:
```javascript
$(document).ready(function(){
  $("h1").css("color","red");
})
```
等同于将&lt;script src="...""></script>在</body>之前引用, 即<body>部分的最后引用. 而不同于在<head>中引用

添加一个css中的class, 改变字体:
```javascript
$("h1").addClass("big-title");
```
删除一个class
```javascript
$("h1").removeClass("big-title");
```
添加两个class
```javascript
$("h1").addClass("big-title margin-50");
```
查看对象是否属于某个class, 返回值为boolean
```javascript
$("h1").hasClass("margin-50");
```


jQuery中更改文本:
```javascript
$("h1").text("morning");
```
或者更改innerHTML:
```JavaScript
$("h1").html("<em>morning</em>");
```

获取attribute 参数:  
```javascript
$("img").attr("src");  //img链接向谁?  
$("img").attr("class");    //img 所属class是?
```  
设置attribute 参数, <a>指向 google.com  
```javascript
$("a").attr("href", "<http://googlw.com">);
```  

jQuery设置Event listener:  
```javascript
$("h1").click(function(){
  $("h1").css("color", "blue");
});
```

jQuery为多个button设置event listener时, 不需要for语句遍历  
```javascript
$("button").click(function(){
  $("h1").css("color", "blue");
});
```

鼠标移动至目标上方时, callback函数:  
```javascript
$("h1").on("mouseover", function(){
  $("h1").text("lll");
});
```

使用jQuery在html段中添加或删除元素. 原句: `<h1>Hello</h1>`  
4种添加:  
```javascript
$("h1").before("<button>click</button>"); //将会变成:
<button>click</button><h1>Hello</h1>
```  

```javascript
$("h1").after("<button>click</button>");//将会变成:
<h1>Hello</h1><button>click</button>
```  

```javascript
$("h1").prepend("<button>click</button>");//将会变成:
<h1><button>click</button>Hello</h1>
```  

```javascript
$("h1").append("<button>click</button>");//将会变成:
<h1>Hello<button>click</button></h1>
```  

1种删除:
```javascript
$("h1").remove("button");
```

jQuery的一些动画操作 配合EventListener的动作`$("h1").on("mouseover", function(){...});`  
更多搜索jQuery Effect Methods  
隐藏/恢复(触发开关)  
```javascript
$("h1").hide();
$("h1").toggle();
```
淡入淡出:
```javascript
$("h1").fadeOut();
$("h1").fadeIn();
```
划入,划出,触发开关:
```javascript
$("h1").slideUp();
$("h1").slideDown();
$("h1").slideToggle();
```
css上的渐变动画:
```javascript
$("h1").animate({opacity:0.5});
$("h1").animate({margin:20%});
```
连续动画:
```javascript
$("h1").slideUp().fadeDown().animate({opacity:0.5});
```

响应GET时, 如何返回多条html语句:  
使用write(), 再send()  
```javascript
app.get("/", function(req, res) {
  res.write("<p>hello one</p>");
  res.write("<h1>hello two</h1>");
  res.send();
});
```
