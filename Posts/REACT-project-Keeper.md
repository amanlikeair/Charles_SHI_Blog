---
title: 'REACT project: Keeper'
date: 2020-10-05 17:00:57
tags:
- Self Learning
- Note
- Web App
- REACT
- Real Project
categories:
- Developer
- Web App
---

# React实践项目keeper
做一个便签管理的项目
最终效果[这个](https://w00gz.csb.app/)

从[这里](https://codesandbox.io/s/keeper-app-part-1-completed-oplw1?fontsize=14)导出这个项目作为基础.

`npm install`

接下来要做的事情:

- 1. Create a new React app.
- 2. Create a App.jsx component.
- 3. Create a Header.jsx component that renders a <header> element to show the Keeper App name in an `<h1>`.
- 4. Create a Footer.jsx component that renders a <footer> element to show a copyright message in a` <p> `with a dynamically updated year.
- 5. Create a Note.jsx component to show a `<div>` element with a `<h1>` for a title and a `<p>` for the content.

### 步骤1-5

最终效果: [这里](https://codesandbox.io/s/keeper-app-part-1-starting-l1pp6?fontsize=14)

### Props
可以这样定义某个模块中的自定义变量:

```JavaScript
import React from "react";
import ReactDOM from "react-dom";

function Card(props) {
  return (
    <div>
      <h2>{props.name}</h2>
      <img src={props.img} alt="avatar_img" />
      <p>{props.tel}</p>
      <p>{props.email}</p>
    </div>
  );
}

ReactDOM.render(
  <div>
    <h1>My Contacts</h1>
    <Card
      name="Beyonce"
      img="https://blackhistorywall.files.wordpress.com/2010/02/picture-device-independent-bitmap-119.jpg"
      tel="+123 456 789"
      email="b@beyonce.com"
    />
    <Card
      name="Jack Bauer"
      img="https://pbs.twimg.com/profile_images/625247595825246208/X3XLea04_400x400.jpg"
      tel="+7387384587"
      email="jack@nowhere.com"
    />
  </div>,
  document.getElementById("root")
);
```

Props之于React, 类似于Attribute之于HTML,

对比:

|Props|React|
|---|---|
|`document.getElementById("email")`<br>`.id`<br>`.placeholder`<br>`.value`|`<input`<br>` id = "email",`<br>` placeholder = "email here",`<br>` value = "123@gmail.com"`<br>`/>`|

|Props|React|
|---|---|
|`function Card(props) {`<br>`return (`<br>`<div>`<br>`<h2>{props.name}</h2>`<br>`<p>{props.tel}</p>`<br>`<p>{props.email}</p>`<br>`</div>`<br>`);`<br>`}`|`<Card`<br>`name="Beyonce"`<br>`tel="+123 456 789"`<br>`email="b@beyonce.com"`<br>`/>`|

一顿修改, 直接看最终代码: [这里](https://codesandbox.io/s/react-props-practice-completed-c6fkx?fontsize=14)

# React DevTools
 一个chrome上的插件, [React Dev Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?utm_source=chrome-ntp-icon)

 可以用于查看react项目的树形结构(模块结构)

# Mapping Components

直接看代码 [这里](https://codesandbox.io/s/mapping-components-y6z4c?fontsize=14)

注意, 这里在<Card>模块中设置了一个名为id的变量,这是`{contacts.map(createCard)}`里的map方法所要求的.

(map方法类似于一个loop,遍历查找contacts里的元素,如果有符合createCard()的,便渲染)










```JavaScript

```
