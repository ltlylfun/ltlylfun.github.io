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

## 前言

本笔记不是零基础笔记

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

## useState

### 设置 state 不会更改现有渲染中的变量，但会请求一次新的渲染。

设置 state 的调用是在告诉 React："组件状态已更新，请安排一个新的渲染周期来反映这一变化。"

1. 当你调用 setState 时，React 并不会立即修改当前渲染（即已经生成的虚拟 DOM）中的变量。这意味着，在当前的渲染过程中，你依然看到的是旧的状态值。

2. setState 调用其实是异步的。它会在内部标记组件需要更新，但真正的更新和重新渲染会在稍后的时机发生（通常是在事件处理结束或其他合适的时机）。更新后的状态会在下一次渲染时生效。

3. 当新的渲染触发后，React 会创建一个全新的虚拟 DOM，并使用更新后的 state 生成新的 UI，然后再比较前后两次虚拟 DOM 的变化，并只更新浏览器中实际变动的部分（即“调和”过程）。

总结来说，设置 state 不会修改当前渲染的变量，它只是请求一次新的渲染，在下一个渲染周期中使用新的 state 来重新计算和更新 UI。这种机制有助于保持组件状态的不可变性和渲染的一致性。

### React 会在事件处理函数执行完成之后处理 state 更新。这被称为批处理。

只有在你的事件处理函数及其中任何代码执行完成 之后，UI 才会更新。这种特性也就是 批处理，它会使你的 React 应用运行得更快。它还会帮你避免处理只 ​​ 更新了一部分 state 变量的令人困惑的“半成品”渲染。

React 不会跨 多个 需要刻意触发的事件（如点击）进行批处理——每次点击都是单独处理的。请放心，React 只会在一般来说安全的情况下才进行批处理。这可以确保，例如，如果第一次点击按钮会禁用表单，那么第二次点击就不会再次提交它。

### 要在一个事件中多次更新某些 state，你可以使用 setNumber(n => n + 1) 更新函数。

### 将 state 视为只读的

```
onPointerMove={e => {
  position.x = e.clientX;
  position.y = e.clientY;
}}
```

这段代码直接修改了 上一次渲染中 分配给 position 的对象。但是因为并没有使用 state 的设置函数，React 并不知道对象已更改。所以 React 没有做出任何响应。虽然在一些情况下，直接修改 state 可能是有效的，但并不推荐这么做。你应该把在渲染过程中可以访问到的 state 视为只读的。

在这种情况下，为了真正地 触发一次重新渲染，你需要创建一个新对象并把它传递给 state 的设置函数：

```
onPointerMove={e => {
  setPosition({
    x: e.clientX,
    y: e.clientY
  });
}}
```

通过使用 setPosition，你在告诉 React：

使用这个新的对象替换 position 的值,然后再次渲染这个组件

### 局部 mutation 是可以接受的

像这样的代码是有问题的，因为它改变了 state 中现有的对象：

```
position.x = e.clientX;
position.y = e.clientY;
```

但是像这样的代码就 没有任何问题，因为你改变的是你刚刚创建的一个新的对象：

```
const nextPosition = {};
nextPosition.x = e.clientX;
nextPosition.y = e.clientY;
setPosition(nextPosition);
```

事实上，它完全等同于下面这种写法：

```
setPosition({
  x: e.clientX,
  y: e.clientY
});
```

只有当你改变已经处于 state 中的 现有 对象时，mutation 才会成为问题。而修改一个你刚刚创建的对象就不会出现任何问题，因为 还没有其他的代码引用它。改变它并不会意外地影响到依赖它的东西。这叫做“局部 mutation”。你甚至可以 在渲染的过程中 进行“局部 mutation”的操作。这种操作既便捷又没有任何问题！

### 使用展开语法复制对象

下面这行代码修改了上一次渲染中的 state：

```
person.firstName = e.target.value;
```

想要实现你的需求，最可靠的办法就是创建一个新的对象并将它传递给 setPerson。但是在这里，你还需要 把当前的数据复制到新对象中，因为你只改变了其中一个字段：

```
setPerson({
  firstName: e.target.value, // 从 input 中获取新的 first name
  lastName: person.lastName,
  email: person.email
});
```

你可以使用`...`对象展开 语法，这样你就不需要单独复制每个属性。

```
setPerson({
  ...person, // 复制上一个 person 中的所有字段
  firstName: e.target.value // 但是覆盖 firstName 字段
});
```

对于大型表单，将所有数据都存放在同一个对象中是非常方便的——前提是你能够正确地更新它！

请注意`...`展开语法本质是是“浅拷贝”——它只会复制一层。这使得它的执行速度很快，但是也意味着当你想要更新一个嵌套属性时，你必须得多次使用展开语法。

### 更新一个嵌套对象

考虑下面这种结构的嵌套对象：

```
const [person, setPerson] = useState({
  name: 'Niki de Saint Phalle',
  artwork: {
    title: 'Blue Nana',
    city: 'Hamburg',
    image: 'https://i.imgur.com/Sd1AgUOm.jpg',
  }
});
```

如果你想要更新 person.artwork.city 的值，用 mutation 来实现的方法非常容易理解：

```
person.artwork.city = 'New Delhi';
```

但是在 React 中，你需要将 state 视为不可变的！为了修改 city 的值，你首先需要创建一个新的 artwork 对象（其中预先填充了上一个 artwork 对象中的数据），然后创建一个新的 person 对象，并使得其中的 artwork 属性指向新创建的 artwork 对象：

```
const nextArtwork = { ...person.artwork, city: 'New Delhi' };
const nextPerson = { ...person, artwork: nextArtwork };
setPerson(nextPerson);
```

或者，写成一个函数调用：

```
setPerson({
  ...person, // 复制其它字段的数据
  artwork: { // 替换 artwork 字段
    ...person.artwork, // 复制之前 person.artwork 中的数据
    city: 'New Delhi' // 但是将 city 的值替换为 New Delhi！
  }
});
```

### 使用 Immer 编写简洁的更新逻辑

如果你的 state 有多层的嵌套，你或许应该考虑 将其扁平化。但是，如果你不想改变 state 的数据结构，你可能更喜欢用一种更便捷的方式来实现嵌套展开的效果。Immer 是一个非常流行的库，它可以让你使用简便但可以直接修改的语法编写代码，并会帮你处理好复制的过程。通过使用 Immer，你写出的代码看起来就像是你“打破了规则”而直接修改了对象：

```
updatePerson(draft => {
  draft.artwork.city = 'Lagos';
});
```

但是不同于一般的 mutation，它并不会覆盖之前的 state！

尝试使用 Immer:

- 运行 `npm install use-immer` 添加 Immer 依赖
- 用 `import { useImmer } from 'use-immer'` 替换掉 `import { useState } from 'react'`

### 在没有 mutation 的前提下更新数组

在 JavaScript 中，数组只是另一种对象。同对象一样，你需要将 React state 中的数组视为只读的。这意味着你不应该使用类似于 arr[0] = 'bird' 这样的方式来重新分配数组中的元素，也不应该使用会直接修改原始数组的方法，例如 push() 和 pop()。

相反，每次要更新一个数组时，你需要把一个新的数组传入 state 的 setting 方法中。为此，你可以通过使用像 filter() 和 map() 这样不会直接修改原始值的方法，从原始数组生成一个新的数组。然后你就可以将 state 设置为这个新生成的数组。

下面是常见数组操作的参考表。当你操作 React state 中的数组时，你需要避免使用左列的方法，而首选右列的方法：
| 操作 | 避免使用 (会改变原始数组) | 推荐使用 (会返回一个新数组） |
|----------|-------------------------------|--------------------------------|
| 添加元素 | push，unshift | concat，[...arr] 展开语法 |
| 删除元素 | pop，shift，splice | filter，slice |
| 替换元素 | splice，arr[i] = ... 赋值 | map |
| 排序 | reverse，sort | 先将数组复制一份 |

## useRef

### 给你的组件添加 ref

你可以通过从 React 导入 useRef Hook 来为你的组件添加一个 ref：

```
import { useRef } from 'react';
```

在你的组件内，调用 useRef Hook 并传入你想要引用的初始值作为唯一参数。例如，这里的 ref 引用的值是“0”：

```
const ref = useRef(0);
```

useRef 返回一个这样的对象:

```
{
  current: 0 // 你向 useRef 传入的值
}
```

与 state 不同的是，ref 是一个普通的 JavaScript 对象，具有可以被读取和修改的 current 属性。

你可以用 ref.current 属性访问该 ref 的当前值。这个值是有意被设置为可变的，意味着你既可以读取它也可以写入它。就像一个 React 追踪不到的、用来存储组件信息的秘密“口袋”。（这就是让它成为 React 单向数据流的“脱围机制”的原因）

### ref 和 state 的不同之处

|               | useRef                                                  | useState                                                                   |
| ------------- | ------------------------------------------------------- | -------------------------------------------------------------------------- |
| 返回值        | useRef(initialValue) 返回 `{ current: initialValue }`   | useState(initialValue) 返回 `[value, setValue]`                            |
| 触发渲染      | 更改时不会触发重新渲染                                  | 更改时触发重新渲染                                                         |
| 可变性        | 可变 —— 你可以在渲染过程之外修改和更新 `current` 的值。 | “不可变” —— 你必须使用 state 设置函数来修改 state 变量，从而排队重新渲染。 |
| 读取/写入时机 | 你不应在渲染期间读取（或写入） `current` 值。           | 你可以随时读取 state。但是，每次渲染都有自己不变的 state 快照。            |

### 使用 ref 操作 DOM

要访问由 React 管理的 DOM 节点，首先，引入 useRef Hook：

```
import { useRef } from 'react';
```

然后，在你的组件中使用它声明一个 ref：

```
const myRef = useRef(null);
```

最后，将 ref 作为 ref 属性值传递给想要获取的 DOM 节点的 JSX 标签：

```
<div ref={myRef}>
```

useRef Hook 返回一个对象，该对象有一个名为 current 的属性。最初，myRef.current 是 null。当 React 为这个 <div> 创建一个 DOM 节点时，React 会把对该节点的引用放入 myRef.current。然后，你可以从 事件处理器 访问此 DOM 节点，并使用在其上定义的内置浏览器 API。

```
// 你可以使用任意浏览器 API，例如：
myRef.current.scrollIntoView();
```

## useEffect

### 如何编写 Effect

要编写一个 Effect, 请遵循以下三个步骤：

1. **_声明 Effect_**。通常 Effect 会在每次 提交 后运行。
2. **指定 Effect 依赖**。大多数 Effect 应该按需运行，而不是在每次渲染后都运行。例如，淡入动画应该只在组件出现时触发。连接和断开服务器的操作只应在组件出现和消失时，或者切换聊天室时执行。你将通过指定 依赖项 来学习如何控制这一点。
3. **必要时添加清理操作**。一些 Effect 需要指定如何停止、撤销，或者清除它们所执行的操作。例如，“连接”需要“断开”，“订阅”需要“退订”，而“获取数据”需要“取消”或者“忽略”。你将学习如何通过返回一个 清理函数 来实现这些。

### 注意事项

- 每当你的组件渲染时，React 会先更新页面，然后再运行 useEffect 中的代码。换句话说，useEffect **会“延迟”一段代码的运行，直到渲染结果反映在页面上**。

- 默认情况下，Effect 会在 每次 渲染后运行。正因如此，以下代码会陷入死循环：

```
const [count, setCount] = useState(0);
useEffect(() => {
  setCount(count + 1);
});
```

- 没有依赖数组和使用空数组 [] 作为依赖数组，行为是不同的：

```
useEffect(() => {
  // 这里的代码会在每次渲染后运行
});

useEffect(() => {
  // 这里的代码只会在组件挂载（首次出现）时运行
}, []);

useEffect(() => {
  // 这里的代码不但会在组件挂载时运行，而且当 a 或 b 的值自上次渲染后发生变化后也会运行
}, [a, b]);
```

- 为什么依赖数组中可以省略 ref?

  这是因为 ref 具有 稳定 的标识：React 确保你在 每轮渲染中调用同一个 useRef 时，总能获得相同的对象。ref 不会改变，所以它不会导致重新运行 Effect。因此，在依赖数组中它可有可无。

  useState 返回的 set 函数 也具有稳定的标识，因此它们通常也会被省略。如果在省略某个依赖项时 linter 不会报错，那么这么做就是安全的。

  省略始终稳定的依赖项仅在 linter 能“看到”对象是稳定的时候才有效。例如，如果 ref 是从父组件传递过来的，则必须在依赖数组中指定它。这很有必要，因为你无法确定父组件是一直传递相同的 ref，还是根据条件传递不同的 ref。所以，你的 Effect 会依赖于被传递的是哪个 ref。

- React 会在每次 Effect 重新运行之前调用清理函数，并在组件卸载（被移除）时最后一次调用清理函数。
