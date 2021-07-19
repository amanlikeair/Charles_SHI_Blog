[< Home](https://amanlikeair.github.io/Charles_SHI_Blog/)


# REACT.js笔记

> React is an open-source, front end, JavaScript library for building user interfaces or UI components.

便捷工具: [codesandbox](https://codesandbox.io/) 在浏览器内编辑和测试react项目

### 开始: hello world:
以hello world程序开始作为介绍:

index.html
```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>JSX</title>
    <link rel="stylesheet" href="styles.css" />
  </head>

  <body>
    <div id="root"></div>
    <script src="../src/index.js" type="text/javascript"></script>
  </body>
</html>
```
index.js
```javascript
import React from "react";
import ReactDOM from "react-dom";

ReactDOM.render(
  <div>
    <h1>Hello World!</h1>
    <p>This is a paragraph.</p>
  </div>,
  document.getElementById("root")
);
```
在js中import两个重要的包react和react-dom

import相当于之前定的require.

调用ReactDOM.render()  , 渲染到html文件中的名为"root"的<div>

ReactDOM.render()中只能渲染一段html的expression语句,所以用<div>括起来.

这被称为JSX, 即JavaScript XML. JSX 允许在react中写HTML.

不仅可以在js里写html, 还可以在html里再写js:

```JavaScript
import React from "react";
import ReactDOM from "react-dom";

const fName = "An";
const lName = "Y";
const num = 7;

ReactDOM.render(
  <div>
    <h1>Hello {fName + " " + lName}!</h1>
    <p>Your lucky number is {num}</p>
  </div>,
  document.getElementById("root")
);
```


### JSX Attributes and Styling

添加一些css和新元素:

```css
.heading {
  color: red;
}

ul {
  color: blue;
}

.food-img {
  height: 100px;
  width: 100px;
}
```

需要注意:原本在html中管用的一些attribute名称需要修改为适合JSX的, 可查表获得对应.

如 `<h1 name="heading">` 需改为 `<h1 className="heading">`


```JavaScript
import React from "react";
import ReactDOM from "react-dom";

const img = "https://picsum.photos/200";

ReactDOM.render(
  <div>
    <h1 className="heading">My Favourite Foods</h1>
    <img alt="random" src={img + "?grayscale"} />

    <img
      className="food-img"
      alt="bacon"
      src="https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/delish-190621-air-fryer-bacon-0035-landscape-pf-1567632709.jpg?crop=0.645xw:0.967xh;0.170xw,0.0204xh&resize=480:*"
    />
    <img
      className="food-img"
      alt="jamon"
      src="https://images-na.ssl-images-amazon.com/images/I/71lNrnbMXsL._SL1200_.jpg"
    />
    <img
      className="food-img"
      alt="noodles"
      src="https://www.errenskitchen.com/wp-content/uploads/2014/04/quick-and-easy-chinese-noodle-soup3-1.jpg"
    />
  </div>,
  document.getElementById("root")
);
```

也可以不在css中定义styling, 而采用*Inline Styling*:

注意,color: "red"外有两层{}
```JavaScript
ReactDOM.render(
  <h1 style={{color: "red"}}>Hello World!</h1>,
  document.getElementById("root")
);
```

也可以:
```JavaScript
const customStyle = {
  color: "red",
  fontSize: "20px",
  border: "1px solid black"
};

ReactDOM.render(
  <h1 style={customStyle}>Hello World!</h1>,
  document.getElementById("root")
);
```

customStyle可以修改赋值:

```JavaScript
customStyle.color = "blue";
```

### React Components

REACT的特点之一就是模块化, 将一些会被重复利用的代码作为模块重复使用,

继续以上面的项目为例, 进行模块化处理:

components文件夹下将有三个模块:

App.JSX 包括heading和list

Heading.JSX

List.JSX

```JavaScript
import React from "react";
import Heading from "./Heading";
import List from "./List";

function App() {
  return (
    <div>
      <Heading />
      <List />
      <List />
    </div>
  );
}

export default App;
```

```JavaScript
import React from "react";

function Heading() {
  return <h1>My Favourite Foods</h1>;
}

export default Heading;
```

```JavaScript
import React from "react";

function List() {
  return (
    <ul>
      <li>Bacon</li>
      <li>Jamon</li>
      <li>Noodles</li>
    </ul>
  );
}

export default List;
```

index.js主文件中调用渲染APP模块
```JavaScript
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";

ReactDOM.render(<App />, document.getElementById("root"));
```

### Import 和 export

新建一个 自己的 math.js

默认输出pi, 其他输出{ doublePi, triplePi }

```JavaScript
const pi = 3.1415962;

function doublePi() {
  return pi * 2;
}

function triplePi() {
  return pi * 3;
}

export default pi;
export { doublePi, triplePi };

```

在主文件中可以这么调用:

```JavaScript
import React from "react";
import ReactDOM from "react-dom";
import pi, { doublePi, triplePi } from "./math.js";

ReactDOM.render(
  <ul>
    <li>{pi}</li>
    <li>{doublePi()}</li>
    <li>{triplePi()}</li>
  </ul>,
  document.getElementById("root")
);
```
也可以这样:

```JavaScript
import React from "react";
import ReactDOM from "react-dom";
import * as pi from "./math.js";

ReactDOM.render(
  <ul>
    <li>{pi.default}</li>
    <li>{pi.doublePi()}</li>
    <li>{pi.triplePi()}</li>
  </ul>,
  document.getElementById("root")
);
```


# WINDOWS下配置本地React JS环境

最新的node

最新的VS code

VS code推荐安装两个extension: sublime babel和 vscode-icons

新建React项目: cd到目标目录 `npx create-react-app my-app`

完成后, cd到my-app, `npm start` 运行app

vs code打开项目目录

已经默认建立了一些文件,我们对项目文件进行精简:

public下只保留index.html

scr下只保留index.js

精简index .htlm 和 .js :

```JavaScript
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>React App</title>
  </head>
  <body>
    <script src="./src/index.js" type="text/jsx"></script>
    <div id="root"></div>
  </body>
</html>
```

```JavaScript
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(<h>hello wolrd</h>, document.getElementById('root'));
```
