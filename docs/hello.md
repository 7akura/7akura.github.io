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

### 5.useEffect的生命周期：如何精确控制副作用的执行时机（依赖数组）？ 如何正确清理副作用（返回清理函数）？

>1. 挂载阶段，组件首次渲染完毕后，React执行useEffect中定义的副作用函数。

>2. 更新阶段，组件重新渲染时根据依赖项数组的情况来决定是否执行副作用:若数组存在且不为空，那么React会将此时的依赖值和上一次渲染的依赖进行比较：如果有变化则先执行上一次副作用返回的清理函数​​，然后执行本次的副作用函数；如果没变化则跳过这次的副作用执行；如果数组为空，则副作用仅在初始渲染时执行一次；如果数组不存在，则每次渲染都会执行副作用。

>3. 卸载阶段，React 执行该 useEffect​​上一次执行副作用时返回的清理函数​​。

>同一个 useEffect的下一次副作用执行之前（依赖项变化时），执行清理函数。

### 6. useCallback& useMemo  ： 它们是如何通过记忆化优化性能的？在什么场景下真正需要使用它们？（避免过早优化）

核心原理：记忆化 (Memoization)
​​定义：​​ 记忆化是一种优化技术，它通过缓存函数调用的结果或值，并在后续调用时，如果输入（依赖项）没有变化，就直接返回缓存的结果，从而避免重复计算。
​​目的：​​ 减少不必要的计算开销，提高性能。

#### useCallBack:记忆化一个函数引用
```jsx
const memoizedCallBack = useCallBack( 
  () =>{
    //函数逻辑
    doSomething(a,b);
  },
  [a,b]//依赖项数组
)
```
1.在组件首次渲染时，useCallback会创建传入的函数并将其缓存。

2.在后续渲染中，React 会将当前的依赖项数组 ([a, b]) 与上一次渲染时的依赖项数组进行​​浅比较​​ (Object.is或 ===比较)。

​3.​如果依赖项没有变化：​​ useCallback会返回​​上一次缓存的函数引用​​。

​4.​如果依赖项有变化：​​ useCallback会创建并返回一个​​新的函数引用​​，并将新的函数和新的依赖项数组缓存起来。
​​优化点：​​ 避免在每次渲染时都创建新的函数实例（新的引用）。这对于传递给子组件（尤其是用 React.memo优化的子组件）的回调函数非常有用，因为稳定的函数引用可以防止子组件不必要的重新渲染（浅比较 props时函数引用相同）。

useCallBack使用场景：
1.传递给 React.memo/ PureComponent子组件的回调函数：​​
如果父组件渲染时，传递给优化子组件（React.memo或 PureComponent）的回调函数每次都是新的引用（例如内联函数 onClick={() => {...}}），即使函数逻辑没变，子组件的浅比较也会认为 props变了，导致子组件不必要的重新渲染。
使用 useCallback可以稳定函数引用，只有当其依赖项变化时才创建新函数，从而让子组件的浅比较生效，避免不必要的渲染。
```jsx
const Child = React.memo(function Child({ onClick }) {
  console.log('Child rendered');
  return <button onClick={onClick}>Click Me</button>;
});
function Parent() {
  const [count, setCount] = useState(0);
  // 使用 useCallback 稳定 onClick 引用
  const handleClick = useCallback(() => {
    console.log('Clicked');
  }, []); // 空依赖，函数引用永不改变
  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>Parent Count: {count}</button>
      <Child onClick={handleClick} />
    </div>
  );
}
```

2.​​作为其他 Hook 的依赖项：​​
如果一个函数被用作 useEffect、useMemo、useCallback（嵌套）或其他自定义 Hook 的依赖项。
如果这个函数在每次渲染时都重新创建，会导致依赖它的 Hook 频繁重新执行（因为依赖项引用变了）。
使用 useCallback稳定函数引用，可以确保依赖它的 Hook 只在函数逻辑真正改变（依赖项变化）时才重新执行。
```jsx
function Example({ data }) {
  // 没有 useCallback: fetchData 每次渲染都变，导致 useEffect 每次渲染都运行
  // const fetchData = () => { ... };
  // 使用 useCallback: 只在 data 变化时 fetchData 引用变
  const fetchData = useCallback(() => {
    // 使用 data 进行数据获取
  }, [data]);
  useEffect(() => {
    fetchData();
  }, [fetchData]); // useEffect 依赖 fetchData
  // ...
}
```

3.​​在自定义 Hook 中暴露稳定的函数：​​
如果你在编写一个自定义 Hook，并且这个 Hook 会返回一个函数供使用者调用（例如事件处理器、操作方法）。
使用 useCallback包裹这个返回的函数，可以确保使用者在使用时（例如在 useEffect依赖项中）获得稳定的引用，避免使用者侧不必要的重新执行或渲染。

#### useMemo: 记忆化一个计算结果（值）
```jsx
const memoizedValue = useMemo(
  () => computerExpensiveValue(a,b),
  [a,b],//依赖项数组
)
```
1.在组件首次渲染时，useMemo会调用传入的计算函数 (computeExpensiveValue)，将其返回值缓存。

2.在后续渲染中，React 会将当前的依赖项数组 ([a, b]) 与上一次渲染时的依赖项数组进行​​浅比较​​。
​
​3.如果依赖项没有变化：​​ useMemo会直接返回​​上一次缓存的计算结果​​，​​不会重新执行计算函数​​。

​​4.如果依赖项有变化：​​ useMemo会​​重新执行计算函数​​，将新的返回值缓存起来并返回。
​​优化点：​​ 避免在每次渲染时都执行开销较大的计算（如复杂的数据转换、过滤、排序等）。只有当依赖项变化时才重新计算。

useMemo使用场景：
1.性能开销昂贵的计算

2.稳定引用类型值作为 props传递给优化子组件：​​
类似于```useCallback```对函数的作用，```useMemo```可以用于稳定对象或数组的引用。
如果你需要将一个对象或数组作为prop传递给一个用 React.memo或 PureComponent优化的子组件，并且这个对象/数组的内容在父组件渲染时可能没有实际变化（但每次渲染都创建新引用），可以使用 ```useMemo```来返回相同的引用。
```jsx
const Child = React.memo(function Child({ config }) {
  // ...
});
function Parent() {
  const [count, setCount] = useState(0);
  // 使用 useMemo 稳定 config 对象引用
  const config = useMemo(() => ({ option: true }), []); // 空依赖，引用永不改变
  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
      <Child config={config} />
    </div>
  );
}
```

3.避免不必要的重新渲染触发

### 7.为什么 Hook 只能在函数组件顶层调用？
React依赖于Hooks的调用顺序来正确关联每次渲染时的状态。如果在条件、循环或嵌套函数中调用，会导致调用顺序不一致，从而无法正确关联状态。
#### 状态与调用顺序绑定​ & 底层链表结构​

React 内部使用单向链表存储 Hook 状态,通过​​调用顺序​​来追踪 Hook 状态

每次渲染时，Hook 的调用顺序必须严格一致，按顺序"认领"对应链表节点


### 8.React如何通过调用顺序管理状态？
React在内部维护一个Hooks的链表（或数组）。每次渲染时，按照Hooks的调用顺序依次读取和更新链表中的状态。如果顺序改变，就会导致状态错乱。
