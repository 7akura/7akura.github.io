---
# 文章标题
title: 测试
# 可选：文章描述
description: 这是我使用Rspress搭建博客的第一篇文章，与此同时 IG 0-2 AL
# 可选：发布日期
date: 2025-09-14
---

# This is a test

## Test

这是我使用Rspress搭建博客的第一篇文章，与此同时IG 2-0 AL 该篇目下将会记录我最近从React中文网学习React.js的一些见解。


## 快速入门
### 创建和嵌套组件
React应用程序是由<b>组件</b>组成的，而React组件是返回标签的JavaScript函数。

``` jsx
function MyButton() {
  return (
    <button>I'm a button</button>
  );
}
//返回一个button
```
React组件以大写字母开头，而HTML标签必须是小字母开头

### 使用JSX编写标签
以上使用的标签语法被称为<i>JSX</i>。比HTML更加严格：必须闭合标签、组件仅能返回单个JSX标签等。<!--  -->[在线转换器](https://transform.tools/html-to-jsx)

### 添加样式
使用<b>className</b>来为某个CSS指定其class
```jsx
<img className="avatar" />
```

### 显示数据
在JSX中使用大括号 <b>{}</b> 包裹变量以展示给用户。
大括号传递变量，引号传递字符串。
```jsx
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```
### 条件渲染
<b>if-else</b>语句或是 <b>条件?运算符</b>。唯一的区别是条件运算符作用在JSX语句中。不需要else分支时可以使用 <b>逻辑&&语法</b>

### 渲染列表
依赖 JavaScript 的特性，例如 <b>for</b> 循环 和 <b>array实例对象 的 map()</b> 函数 来渲染组件列表。对于列表中的每一个元素，都应该传递一个<b>字符串</b>或者<b>数字</b>给 key，用于在其兄弟节点中唯一标识该元素。不强制要求被渲染的对象具有[Iterator接口](https://es6.ruanyifeng.com/#docs/iterator)

### 响应事件
在组件中声明<b>事件处理函数</b>来响应事件。其中，只需把函数传递给事件，而不是在组件中调用该事件。
```jsx
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }
  //传递而非调用
  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```
### 更新页面
使用useState Hook，它接受两个参数，分别是当前的<b>state</b>以及用于更新state的函数<b>setState()</b>。
如果多次渲染同一个组件，每个组件都会拥有自己的 state。
```jsx
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

### 使用 Hook 
以 use 开头的函数被称为 Hook。useState 是 React 提供的一个内置 Hook。

Hook 比普通函数更为严格。只能在组件（或其他 Hook）的 顶层 调用 Hook。如果想在一个条件或循环中使用 useState，请提取一个新的组件并在组件内部使用它。

### 在组件中共享数据
在上述中提到，多次渲染同一个组件时，每个组件都会拥有自己的state。然而，在实际情况中，我们经常会遇到<b>组件共享数据并一起更新</b>的场景。
此时，我们需要先把组件中定义的 state 与 setState()的事件处理函数 移动到<b>包含所有需要共享数据组件的父组件之中</b>,再将事件处理函数以及 state 一同向下传递到每个组件中。


其中，被传递的 state 与 事件处理函数 被称作为<b>Prop</b>(传参)，同时我们也需要更新组件使其能够接受对应的Prop。

```jsx
import { useState } from 'react';

export default function MyApp() {
//state与setState的事件处理函数上移
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) { //改造MyButton函数使其能接收count与onClick事件两个参数
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```