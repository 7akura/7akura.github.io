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

7azusa 2024/12/9 16:35:04


7azusa 2024/12/18 10:58:26


7azusa 2024/12/19 18:50:09


7azusa 2025/1/4 23:43:11


7azusa 2025/1/22 1:44:07


7azusa 2025/3/9 15:04:28
FWTV-1336【产品详情】V1.2.0-调整【备注】保存接口

7azusa 2025/3/9 15:05:06
FWTV-1299【产品详情】V1.2.0-新增静态交互功能

7azusa 2025/3/9 15:06:18
FWTV-1300【产品详情】V1.2.0-新增产品详情【版本列表】接口

7azusa 2025/3/9 15:06:33
FWTV-1314【产品详情】V1.2.0-新增产品详情接口字段【产品版本】

7azusa 2025/3/9 15:06:52
FWTV-1332【产品详情】V1.2.0-新增产品详情【举报】接口字段【产品版本】

7azusa 2025/3/9 15:11:00
FWTV-1330 【产品详情】V1.2.0-调整【备注】展示字段

7azusa 2025/3/9 22:58:26


7azusa 2025/3/10 22:51:12
朴素的思路是遍历数组 nums 的每个大小至少为 2 的子数组并计算每个子数组的元素和，判断是否存在一个子数组的元素和为 k 的倍数。当数组 nums 的长度为 m 时，上述思路需要用 O(m 
2
 ) 的时间遍历全部子数组，对于每个子数组需要 O(m) 的时间计算元素和，因此时间复杂度是 O(m 
3
 )，会超出时间限制，因此必须优化。

如果事先计算出数组 nums 的前缀和数组，则对于任意一个子数组，都可以在 O(1) 的时间内得到其元素和。用 prefixSums[i] 表示数组 nums 从下标 0 到下标 i 的前缀和，则 nums 从下标 p+1 到下标 q（其中 p<q）的子数组的长度为 q−p，该子数组的元素和为 prefixSums[q]−prefixSums[p]。

如果 prefixSums[q]−prefixSums[p] 为 k 的倍数，且 q−p≥2，则上述子数组即满足大小至少为 2 且元素和为 k 的倍数。

当 prefixSums[q]−prefixSums[p] 为 k 的倍数时，prefixSums[p] 和 prefixSums[q] 除以 k 的余数相同。因此只需要计算每个下标对应的前缀和除以 k 的余数即可，使用哈希表存储每个余数第一次出现的下标。

规定空的前缀的结束下标为 −1，由于空的前缀的元素和为 0，因此在哈希表中存入键值对 (0,−1)。对于 0≤i<m，从小到大依次遍历每个 i，计算每个下标对应的前缀和除以 k 的余数，并维护哈希表：

如果当前余数在哈希表中已经存在，则取出该余数在哈希表中对应的下标 prevIndex，nums 从下标 prevIndex+1 到下标 i 的子数组的长度为 i−prevIndex，该子数组的元素和为 k 的倍数，如果 i−prevIndex≥2，则找到了一个大小至少为 2 且元素和为 k 的倍数的子数组，返回 true；

如果当前余数在哈希表中不存在，则将当前余数和当前下标 i 的键值对存入哈希表中。

由于哈希表存储的是每个余数第一次出现的下标，因此当遇到重复的余数时，根据当前下标和哈希表中存储的下标计算得到的子数组长度是以当前下标结尾的子数组中满足元素和为 k 的倍数的子数组长度中的最大值。只要最大长度至少为 2，即存在符合要求的子数组。

7azusa 2025/3/12 0:06:13
https://qishui.douyin.com/s/i5gyEb3C/

7azusa 2025/3/12 0:06:47
看前面 我忘记了是哪个夏天

你轻靠着我 飘散而过的落叶

为了誓言 让时间延伸就像永远

迟钝如我 也感觉到的边缘

在思念的空间里不断徘徊

那距离却越明显

持续的提醒我 现实的界限

又一遍 我忘记了是哪些时间

你言辞闪烁 原因当然不明显

试着看见 让时间倒转回到从前

认真如我 有抓不到的边缘

在想象的空间里不断徘徊

那画面永远明确

就算是闭上眼 也无法否决

我怎么会让自己舍身不断涉险

你怎么会对我的心不断地拒绝

爱失去你的包围

每次退后又错过你的世界一点

我没有办法清醒应付新的对决

你却轻易让我的心委屈到极限

爱有了你 却失去了我的一切

衡量你的心直线到我之间

没有跨越的机会

又一遍 我忘记了是哪些时间

你言辞闪烁 原因当然不明显

试着看见 让时间倒转回到从前

认真如我 有抓不到的边缘

在想象的空间里不断徘徊

让画面永远明确

就算是闭上眼 也无法否决

我怎么会让自己舍身不断涉险

你怎么会对我的心不断地拒绝

爱失去你的包围

每次退后又错过你的世界一点

我没有办法清醒应付新的对决

你却轻易让我的心委屈到极限

爱有了你 却失去了我的一切

衡量你的心直线到我之间

没有跨越的机会

没有跨越的机会

no衡量你的心直线到我之间

没有跨越的机会no no no

看前面 我忘记了是哪个夏天

7azusa 2025/3/31 6:08:02


7azusa 2025/4/6 21:12:06
[Allen的聊天记录]

7azusa 2025/4/8 16:47:57
https://lol.qq.com/act/a20250220makedjusticepass/index-m.html

7akura1 2025/8/20 21:43:16
460028197107190013

7akura1 0:54:58

## Q&A
### 1.虚拟DOM  ： React是如何通过虚拟DOM高效更新UI的？keyprop在Diff过程中起到了什么关键作用？

>操作虚拟Dom(JavaScript对象)的成本要低于直接操作真实Dom(浏览器的Dom树)的成本，操控真实Dom可能会导致回流、重绘、合成等操作；上下文切换开销(进程或线程不同)；React的批处理机制确保了在一次事件处理函数中对state的多次更新只触发一次重渲染，即通过diff算法得出需要更新的部分，再去更新真实Dom，保证最少的重渲染次数。</li>

>key prop 是 React 用于在 Diff 算法中识别元素的唯一标识符。当 React 进行元素的比较时，它会根据 key prop 来判断元素是否发生了变化。如果 key prop 没有发生变化，React 会假设元素没有发生变化，从而避免不必要的重新渲染。

### 2.diff算法的策略？
>React 的 Diff 算法采用了以下策略：
>1. 同级比较：只比较同一层级的节点，避免跨层级比较。
>2. 递归比较：如果是组件节点，递归比较组件的子节点。
>3. 列表比较：如果是列表节点，根据 key prop 进行比较，找到差异的部分进行更新。
>4. 其他情况：如果以上策略都不匹配，React 会假设节点发生了变化，从而进行更新。

### 3.状态不可变性  ： 为什么State不能直接修改？setState/useState的更新是同步还是异步？什么是“批量更新”？
>React 依赖于状态引用来判断组件是否需要更新。当你调用 setState或 useState的 setter 函数时，React 会​​浅比较​​新旧状态的引用
>如果引用相同，React 会认为状态没有变化，从而跳过更新过程。如果你直接修改状态对象的属性，引用不会改变，React 将无法检测到变化，导致组件不会重新渲染。

>同时，React 提供了 PureComponent（类组件）和 React.memo（函数组件）来帮助进行性能优化。它们默认会对组件的 props和 state进行​​浅比较​​，如果都没有变化，则跳过渲染。若直接修改State则会导致浅比较失效，从而导致React错误地认为没有变化，导致组件不进行更新。

>setState（类组件）和 useState的 setter 函数（函数组件）的更新是异步的。这意味着在调用 setState或 useState的 setter 函数后，React 不会立即更新状态，而是会将更新添加到一个队列中。React 会在事件处理函数执行完成之后、重新渲染之前处理状态更新队列。这被称为批量更新。批量更新可以确保在一次事件处理函数中对状态的多次更新只触发一次重新渲染，从而提高性能。

### 4.组件渲染机制  ： 父组件状态更新会导致所有子组件重新渲染吗？如何避免不必要的子组件渲染？
> 父组件状态更新会触发其所有子组件的​​渲染函数执行​​（JS 计算），但 Diff 算法会阻止不必要的​​真实 DOM 更新​​

 如何避免不必要的子组件渲染？
>1. 使用 React.memo 或 PureComponent 对子组件进行包装，以进行 props 或 state 的浅比较，避免不必要的渲染。
>2. 避免在子组件中直接修改 props 或 state，而应该通过父组件传递的 setter 函数进行更新。
>3. 使用 useCallback和 useMemo缓存函数引用，避免父组件渲染时重新创建函数导致子组件不必要的渲染。
>4. 使用 shouldComponentUpdate 生命周期方法（类组件）或 React.memo 的比较函数（函数组件）来手动控制是否渲染子组件。

