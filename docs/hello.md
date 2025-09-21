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
### 保持组件纯粹
##### 纯函数:组件作为公式
纯函数 通常具有如下特征：

<b>只负责自己的任务</b>。它不会更改在该函数调用前就已存在的对象或变量。
<b>输入相同，则输出相同</b>。给定相同的输入，纯函数应总是返回相同的结果。

React 便围绕着这个概念进行设计。React 假设你编写的所有组件都是纯函数。也就是说，对于相同的输入，你所编写的 React 组件必须总是返回相同的 JSX。
React 的渲染过程必须自始至终是纯粹的。组件应该只 返回 它们的 JSX，而不 改变 在渲染前，就已存在的任何对象或变量 
否则多次调用这个组件会产生不同的 JSX！并且，如果 其他 组件读取 guest ，它们也会产生不同的 JSX，其结果取决于它们何时被渲染（可以通过把对象/参数作为Prop传入来修复）

局部 mutation：
组件改变了预先存在的 变量的值。这种现象称为 突变（mutation
纯函数不会改变函数作用域外的变量、或在函数调用前创建的对象。但你可以在渲染时更改你 刚刚 创建的变量和对象，通过Prop传入纯函数组件中。利用纯函数不会影响外部变量的特性来保证每次输入的Prop的值是可控的，即为局部mutation

### 将ui视为树
<b>树是项目和ui之间之间的关系模型</b>，通常使用树结构来表示UI。例如，浏览器使用树结构来建模 HTML（DOM）与CSS（CSSOM），最后形成render tree.....() 移动平台也使用树来表示其视图层次结构。

#### 渲染树
React 使用树结构来管理和建模 React 应用程序中组件之间的关系。在 React 渲染树中，根节点是应用程序的 根组件，树中的每个箭头从父组件指向子组件。
渲染树有助于识别 React 应用程序中的顶级和叶子组件。顶级组件是离根组件最近的组件，它们影响其下所有组件的渲染性能，通常包含最多复杂性。叶子组件位于树的底部，没有子组件，通常会频繁重新渲染。识别这些组件类别有助于理解应用程序的数据流和性能。

#### 模块依赖树 
依赖树对于确定运行 React 应用程序所需的模块非常有用。在为生产环境构建 React 应用程序时，通常会有一个构建步骤，该步骤将捆绑所有必要的 JavaScript 以供客户端使用。负责此操作的工具称为 bundler（捆绑器），并且 bundler 将使用依赖树来确定应包含哪些模块。
## 添加交互
### 响应事件
在组件内，先定义一个函数，然后 将其作为 ```prop``` 传入需要实现逻辑的 JSX 标签
```jsx
export default function Button() {
  function handleClick() {
    alert('你点击了我！');
  }

  return (
    <button onClick={handleClick}>
      点我
    </button>
  );
}
```
其中 ```handleClick``` 是一个<b>事件处理函数</b>,在以上代码中被作为```Prop``` 传递给了```<button>``` 标签。
事件处理函数有如下特点:
<ul>
<li>通常在你的组件<b>内部</b>定义。
<li>名称以 handle 开头，后跟事件名称。
</ul>  
或者，你也可以在 JSX 中定义一个内联的事件处理函数：

```jsx
export default function Button() {
  return (
    <button onClick={() => alert('你点击了我！')}>
      点我
    </button>
  );
}
```
或者，直接使用更为简洁箭头函数：
```jsx
<button onClick={() => {
  alert('你点击了我！');
}}>
```
以上几种方式均是等效的，随你喜好选择即可。

### 事件传播
事件处理函数还将捕获任何来自子组件的事件。通常，我们会说事件会沿着树向上“冒泡”或“传播”：它从事件发生的地方开始，然后沿着树<b>向上传播</b>。

在 React 中所有事件都会传播，除了 onScroll，它仅适用于你附加到的 JSX 标签。
#### 阻止传播 
事件处理函数接收一个 事件对象 作为唯一的参数。按照惯例，它通常被称为 e ，代表 “event”（事件）。你可以使用此对象来读取有关事件的信息。

这个事件对象还允许你阻止传播。如果你想阻止一个事件到达父组件，可以调用 ```e.stopPropagation()```。

#### 阻止默认行为
某些浏览器事件具有与事件相关联的默认行为，例如点击链接时会导航到新页面，提交表单时会刷新页面等。如果想阻止这些默认行为，可以调用 ```e.preventDefault()```。

不要混淆 e.stopPropagation() 和 e.preventDefault()。它们都很有用，但二者并不相关：
<ul>
<li>e.stopPropagation() 阻止触发绑定在外层标签上的事件处理函数。
<li>e.preventDefault() 阻止少数事件的默认浏览器行为
</ul>

### state 与 setState()
<ol>
<li>局部变量无法在多次渲染中持久保存。 当 React 再次渲染这个组件时，它会从头开始渲染——不会考虑之前对局部变量的任何更改
<li>更改局部变量不会触发渲染。 React 没有意识到它需要使用新数据再次渲染组件。

</ol>
要使用新数据更新组件，需要做两件事：
<ol>
<li>保留 渲染之间的数据。
<li>触发 React 使用新数据渲染组件（重新渲染）。
</ol>
useState Hook 提供了这两个功能：
<ol>
<li>State 变量 用于保存渲染间的数据。
<li>State setter 函数 更新变量并触发 React 再次渲染组件。
</ol>

### 渲染与提交
#### 1.触发渲染
有两种原因会导致组件的渲染：
<ol>
<li>组件的<b>初次渲染</b>(通过调用createRoot方法并传入目标DOM节点，)
<li>组件（或者其祖先之一）的<b>状态发生了改变</b>（使用set函数更新状态）
</ol>

#### 2.React渲染
在你触发渲染后，React 会调用你的组件来确定要在屏幕上显示的内容。“渲染中” 即 React 在调用你的组件。

在进行初次渲染时, React 会调用根组件。
对于后续的渲染, React 会调用内部状态更新触发了渲染的函数组件。

#### 3.React 把更改提交到 DOM 上
在渲染（调用）你的组件之后，React 将会修改 DOM。

对于初次渲染，React 会使用 appendChild() DOM API 将其创建的所有 DOM 节点放在屏幕上。
对于重渲染，React 将应用最少的必要操作（在渲染时计算！ By diff树？），以使得 DOM 与最新的渲染输出相互匹配。

<b>React 仅在渲染之间存在差异时才会更改 DOM 节点。</b>

<b>设置 state 只会为下一次渲染变更 state 的值</b>

React 会使 state 的值始终“固定”在一次渲染的各个事件处理函数内部。在下一次渲染之前，从state获取的数据总是相同的。但我们可以通过批量地使用set函数来更改state里的值，保证下一次渲染时state的值是正确的。设置 state 不会更改现有渲染中的变量，但会请求一次新的渲染。React 会在事件处理函数执行完成之后处理 state 更新。这被称为批处理。批量处理set函数后，React才会提交因该次state的修改产生的重渲染

### 控制state
#### 将state视为只读的
不要去直接修改state里的值，而应该通过set函数去更换state里的值。使用扩展运算符```...```去创建一个新的state副本，在副本中修改数据再去通过set函数更新。

#### 组件共享state
进行<b>状态提升</b>。将相关 state 从这两个组件上移除，并把 state 放到它们的公共父级，再通过 props 将 state 传递给这两个组件。
考虑该将组件视为“受控”（由 prop 驱动）或是“不受控”（由 state 驱动）是十分有益的。

#### 对state进行保留和重置
各个组件的 state 是各自独立的。根据组件在 UI 树中的位置，React 可以跟踪哪些 state 属于哪个组件。你可以控制在重新渲染过程中何时对 state 进行保留和重置。
<ul>
<li>相同位置的相同组件会使得 state 被保留下来
<li>相同位置的不同组件会使 state 重置
<li>在相同位置重置 state 
  <ol>
    <li>方法一：将组件渲染在不同的位置
    <li>方法二：使用 key 来重置 state
  </ol>
</ul>

### Reducer
对于拥有许多状态更新逻辑的组件来说，过于分散的事件处理程序可能会令人不知所措。对于这种情况，你可以将组件的所有状态更新逻辑整合到一个外部函数中，这个函数叫作 reducer。

将原本的set函数内修改state的逻辑转变为 dispatch 一个action传递给reducer函数。在reducer函数内去修改state。useReducer 和 useState 很相似——你必须给它传递一个初始状态，它会返回一个有状态的值和一个设置该状态的函数（dispatch）。

### Context
Context 使组件向其下方的整个树提供信息。可以跨组件进行通信

通过 ```export const MyContext = createContext(defaultValue)``` 创建并导出 context。

在无论层级多深的任何子组件中，把 context 传递给 ```useContext(MyContext)``` Hook 来读取它。

在父组件中把 children 包在 ```<MyContext.Provider value={...}>```中来提供 context。

## 应急方案
### 使用 ref 引用值
当你希望组件“记住”某些信息，但又不想让这些信息<b>触发新的渲染</b> 时，你可以使用 <b>useRef</b> Hook
与state一样，ref在重新渲染之间由 React 保留。但是，设置state会重新渲染组件，而更改ref不会

### 使用 ref 操作 DOM

### 使用 Effect 实现同步