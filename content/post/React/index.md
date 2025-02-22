---
title: 关于React的笔记
description: 记录一些React笔记
date: 2025-02-22T14:20:09+08:00
comments: true
keywords: [React]
categories:
  - React
tags:
  - React
---

## React 组件名称

React 组件是常规的 JavaScript 函数，但 组件的名称必须以**大写字母开头**，否则它们将无法运行！

## jsx 的规则

### jsx 的 return

这个组件返回一个带有 src 和 alt 属性的 <img /> 标签。<img /> 写得像 HTML，但实际上是 JavaScript！这种语法被称为 JSX，它允许你在 JavaScript 中嵌入标签。

返回语句可以全写在一行上，如下面组件中所示：

```jsx
return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;
```

但是，如果你的标签和 return 关键字不在同一行，则必须把它包裹在一对括号中，如下所示：

```jsx
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

**没有括号包裹的话，任何在 return 下一行的代码都 将被忽略！**

JSX 虽然看起来很像 HTML，但在底层其实被转化为了 JavaScript 对象，你**不能在一个函数中返回多个对象，除非用一个数组把他们包装起来**。这就是为什么多个 JSX 标签必须要用一个父元素或者 Fragment 来包裹。

### 标签必须闭合

JSX 要求标签必须正确闭合。像 `<img>` 这样的自闭合标签必须书写成 `<img />`，而像 `<li>oranges` 这样只有开始标签的元素必须带有闭合标签，需要改为 `<li>oranges</li>`。

### 使用驼峰式命名法给 所有 大部分属性命名！

JSX 最终会被转化为 JavaScript，而 JSX 中的属性也会变成 JavaScript 对象中的键值对。在你自己的组件中，经常会遇到需要用变量的方式读取这些属性的时候。但 JavaScript 对变量的命名有限制。例如，变量名称不能包含 - 符号或者像 class 这样的保留字。

这就是为什么在 React 中，大部分 HTML 和 SVG 属性都用驼峰式命名法表示。例如，需要用 strokeWidth 代替 stroke-width。由于 class 是一个保留字，所以在 React 中需要用 className 来代替。这也是 DOM 属性中的命名。

## 默认导出与具名导出

| 语法 | 导出语句                              | 导入语句                                |
| ---- | ------------------------------------- | --------------------------------------- |
| 默认 | `export default function Button() {}` | `import Button from './Button.js';`     |
| 具名 | `export function Button() {}`         | `import { Button } from './Button.js';` |

一个文件里有且仅有一个 默认 导出，但是可以有任意多个 具名 导出。

当使用默认导入时，你可以在 import 语句后面进行任意命名。比如 import Banana from './Button.js'，如此你能获得与默认导出一致的内容。相反，对于具名导入，导入和导出的名字必须一致。这也是称其为 具名 导入的原因！

通常，文件中仅包含一个组件时，人们会选择默认导出，而当文件中包含多个组件或某个值需要导出时，则会选择具名导出。 无论选择哪种方式，请记得给你的组件和相应的文件命名一个有意义的名字。不建议创建未命名的组件，比如 export default () => {}，因为这样会使得调试变得异常困难。

## 在大括号 { } 中使用 JavaScript

### 在 JSX 中，只能在以下两种场景中使用大括号

用作 JSX 标签内的文本：`<h1>{name}'s To Do List</h1>`是有效的，但是 `<{tag}>Gregorio Y. Zara's To Do List</{tag}>` 无效。
用作紧跟在 = 符号后的 属性：`src={avatar}` 会读取 `avatar` 变量，但是 `src="{avatar}"` 只会传一个字符串 `{avatar}`。

### 使用 “双大括号”：JSX 中的 CSS 和 对象

除了字符串、数字和其它 JavaScript 表达式，你甚至可以在 JSX 中传递对象。对象也用大括号表示，例如 `{ name: "Hedy Lamarr", inventions: 5 }`。因此，为了能在 JSX 中传递，你必须用另一对额外的大括号包裹对象：`person={{ name: "Hedy Lamarr", inventions: 5 }}`。

你可能在 JSX 的内联 CSS 样式中就已经见过这种写法了。React 不要求你使用内联样式（使用 CSS 类就能满足大部分情况）。但是当你需要内联样式的时候，你可以给 style 属性传递一个对象：

```
export default function TodoList() {
  return (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Improve the videophone</li>
      <li>Prepare aeronautics lectures</li>
      <li>Work on the alcohol-fuelled engine</li>
    </ul>
  );
}

```

注意，内联 style 属性 使用驼峰命名法编写。例如，HTML `<ul style="background-color: black">` 在你的组件里应该写成 `<ul style={{ backgroundColor: 'black' }}>`。

所以当你下次在 JSX 中看到 `{{    }}`时，就知道它只不过是包在大括号里的一个对象罢了！

## props

- 要传递 props，请将它们添加到 JSX，就像使用 HTML 属性一样。
- 要读取 props，请使用 `function Avatar({ person, size })` 解构语法。
- 你可以指定一个默认值，如 size = 100，用于缺少值或值为 undefined 的 props 。
- 你可以使用 `<Avatar {...props} /> `JSX 展开语法转发所有 props，但不要过度使用它！
- 像 `<Card><Avatar /></Card>` 这样的嵌套 JSX，将被视为 Card 组件的 children prop。
- Props 是只读的时间快照：每次渲染都会收到新版本的 props。
- 你不能改变 props。当你需要交互性时，你可以设置 state。

## 条件渲染

- 在 React，你可以使用 JavaScript 来控制分支逻辑。
- 你可以使用 if 语句来选择性地返回 JSX 表达式。
- 你可以选择性地将一些 JSX 赋值给变量，然后用大括号将其嵌入到其他 JSX 中。
- 在 JSX 中，`{cond ? <A /> : <B />}` 表示 “当 cond 为真值时, 渲染 `<A />`，否则 `<B />`”。
- 在 JSX 中，`{cond && <A />}` 表示 “当 cond 为真值时, 渲染 <A />，否则不进行渲染”。
- 快捷的表达式很常见，但如果你更倾向于使用 if，你也可以不使用它们。

## key

### 如何设定 key 值

不同来源的数据往往对应不同的 key 值获取方式：

来自数据库的数据： 如果你的数据是从数据库中获取的，那你可以直接使用数据表中的主键，因为它们天然具有唯一性。
本地产生数据： 如果你数据的产生和保存都在本地（例如笔记软件里的笔记），那么你可以使用一个自增计数器或者一个类似 uuid 的库来生成 key。

### key 需要满足的条件

key 值在兄弟节点之间必须是唯一的。 不过不要求全局唯一，在不同的数组中可以使用相同的 key。

key 值不能改变，否则就失去了使用 key 的意义！所以千万不要在渲染时动态地生成 key。

### React 中为什么需要 key？

设想一下，假如你桌面上的文件都没有文件名，取而代之的是，你需要通过文件的位置顺序来区分它们———第一个文件，第二个文件，以此类推。也许你也不是不能接受这种方式，可是一旦你删除了其中的一个文件，这种组织方式就会变得混乱无比。原来的第二个文件可能会变成第一个文件，第三个文件会成为第二个文件……

React 里需要 key 和文件夹里的文件需要有文件名的道理是类似的。它们（key 和文件名）都让我们可以从众多的兄弟元素中唯一标识出某一项（JSX 节点或文件）。而一个精心选择的 key 值所能提供的信息远远不止于这个元素在数组中的位置。即使元素的位置在渲染的过程中发生了改变，它提供的 key 值也能让 React 在整个生命周期中一直认得它。

## 保持组件纯粹

- 一个组件必须是纯粹的，就意味着：

  - 只负责自己的任务。 它不会更改在该函数调用前就已存在的对象或变量。
  - 输入相同，则输出相同。 给定相同的输入，组件应该总是返回相同的 JSX。

- 渲染随时可能发生，因此组件不应依赖于彼此的渲染顺序。
- 你不应该改变任何用于组件渲染的输入。这包括 props、state 和 context。通过 “设置” state 来更新界面，而不要改变预先存在的对象。
- 努力在你返回的 JSX 中表达你的组件逻辑。当你需要“改变事物”时，你通常希望在事件处理程序中进行。作为最后的手段，你可以使用 useEffect。

## 传递给事件的函数

| 传递一个函数（正确）             | 调用一个函数（错误）               |
| -------------------------------- | ---------------------------------- |
| `<button onClick={handleClick}>` | `<button onClick={handleClick()}>` |

区别很微妙。在第一个示例中，handleClick 函数作为 onClick 事件处理函数传递。这会让 React 记住它，并且只在用户点击按钮时调用你的函数。

在第二个示例中，handleClick() 中最后的 () 会在 渲染 过程中 立即 触发函数，即使没有任何点击。这是因为位于 JSX {} 之间的 JavaScript 会立即执行。

| 传递一个函数（正确）                    | 调用一个函数（错误）              |
| --------------------------------------- | --------------------------------- |
| `<button onClick={() => alert('...')}>` | `<button onClick={alert('...')}>` |

如果按如下方式传递内联代码，并不会在点击时触发，而是会在每次组件渲染时触发：

```
// 这个 alert 在组件渲染时触发，而不是点击时触发！
<button onClick={alert('你点击了我！')}>
```

如果你想要定义内联事件处理函数，请将其包装在匿名函数中，如下所示：

```
<button onClick={() => alert('你点击了我！')}>
```

这里创建了一个稍后调用的函数，而不会在每次渲染时执行其内部代码。

在这两种情况下，你都应该传递一个函数：

- `<button onClick={handleClick}>` 传递了 handleClick 函数。
- `<button onClick={() => alert('...')}>` 传递了 () => alert('...') 函数。

## 事件的传播

在 React 中所有事件都会传播，除了 onScroll，它仅适用于你附加到的 JSX 标签。

## 阻止传播

事件处理函数接收一个 事件对象 作为唯一的参数。按照惯例，它通常被称为 e ，代表 “event”（事件）。你可以使用此对象来读取有关事件的信息。

这个事件对象还允许你阻止传播。如果你想阻止一个事件到达父组件，你需可以调用 e.stopPropagation() ：

```
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
}

export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('你点击了 toolbar ！');
    }}>
      <Button onClick={() => alert('正在播放！')}>
        播放电影
      </Button>
      <Button onClick={() => alert('正在上传！')}>
        上传图片
      </Button>
    </div>
  );
}
```

## 阻止默认行为

某些浏览器事件具有与事件相关联的默认行为。例如，点击 <form> 表单内部的按钮会触发表单提交事件，默认情况下将重新加载整个页面

你可以调用事件对象中的 e.preventDefault() 来阻止这种情况发生

```
export default function Signup() {
  return (
    <form onSubmit={e => {
      e.preventDefault();
      alert('提交表单！');
    }}>
      <input />
      <button>发送</button>
    </form>
  );
}
```
