---
title: 关于CSS的笔记
description: 记录一些CSS笔记
date: 2025-02-15T16:35:09+08:00
comments: true
keywords: [CSS]
categories:
  - CSS-Notes
tags:
  - CSS
---

## 前言

在前端开发中，CSS（层叠样式表）是一门非常重要的语言，它控制着网页的视觉呈现效果。作为前端三大基础技术（HTML、CSS、JavaScript）之一，掌握 CSS 对于构建优美的用户界面至关重要。

希望这些笔记不仅能帮助自己梳理知识体系，也能为其他学习 CSS 的朋友提供参考。

PS 不定时更新，欢迎讨论区留言。

## CSS 选择器及其优先级

1. 优先级权重表

| 选择器类型   | 权重值  | 示例                              | 说明                 |
| ------------ | ------- | --------------------------------- | -------------------- |
| !important   | ∞       | `color: red !important;`          | 最高优先级           |
| 行内样式     | 1,0,0,0 | `<div style="...">`               | 直接在标签上的样式   |
| ID 选择器    | 0,1,0,0 | `#header`                         | 以#开头的选择器      |
| 类/伪类/属性 | 0,0,1,0 | `.class` `:hover` `[type="text"]` | 类、伪类、属性选择器 |
| 元素/伪元素  | 0,0,0,1 | `div` `::before`                  | 标签选择器和伪元素   |
| 通配符       | 0,0,0,0 | `*`                               | 最低优先级           |

2. 常用选择器示例表

| 选择器类型 | 语法          | 示例                                   | 说明                   |
| ---------- | ------------- | -------------------------------------- | ---------------------- |
| 通配符     | `*`           | `* { margin: 0; }`                     | 选择所有元素           |
| 元素选择器 | `element`     | `p { color: blue; }`                   | 选择指定标签           |
| 类选择器   | `.class`      | `.active { color: red; }`              | 选择指定类名           |
| ID 选择器  | `#id`         | `#header { height: 60px; }`            | 选择指定 ID            |
| 属性选择器 | `[attribute]` | `[type="text"] { border: 1px solid; }` | 选择带有特定属性的元素 |

3. 组合选择器表

| 组合方式   | 语法    | 示例                       | 优先级计算    |
| ---------- | ------- | -------------------------- | ------------- |
| 后代选择器 | `A B`   | `.nav a { color: blue; }`  | 累加(0,0,1,1) |
| 子选择器   | `A > B` | `.nav > a { color: red; }` | 累加(0,0,1,1) |
| 相邻兄弟   | `A + B` | `h2 + p { margin: 10px; }` | 累加(0,0,0,2) |
| 通用兄弟   | `A ~ B` | `h2 ~ p { color: gray; }`  | 累加(0,0,0,2) |

4. 伪类和伪元素表

| 类型   | 示例           | 优先级  | 说明           |
| ------ | -------------- | ------- | -------------- |
| 伪类   | `:hover`       | 0,0,1,0 | 特定状态的样式 |
| 伪类   | `:first-child` | 0,0,1,0 | 位置相关的样式 |
| 伪元素 | `::before`     | 0,0,0,1 | 创建额外的元素 |
| 伪元素 | `::first-line` | 0,0,0,1 | 特殊的文本样式 |

5. 复杂选择器优先级计算表

| 选择器           | 计算方式                          | 最终优先级 | 说明         |
| ---------------- | --------------------------------- | ---------- | ------------ |
| `.header.active` | (0,0,1,0) + (0,0,1,0)             | 0,0,2,0    | 多类选择器   |
| `#nav .list li`  | (0,1,0,0) + (0,0,1,0) + (0,0,0,1) | 0,1,1,1    | ID+类+元素   |
| `.nav a:hover`   | (0,0,1,0) + (0,0,0,1) + (0,0,1,0) | 0,0,2,1    | 类+元素+伪类 |

## flex

![flex布局](flex_terms.png)

CSS Flexbox（弹性盒布局）是一种用于在容器内分布空间以及对齐内容的布局模式。它提供了一种更高效的方式来排列、对齐和分布容器内的项目，即使在复杂的应用或大型屏幕设备上也能很好地工作。

- **容器（Flex Container）**：使用 `display: flex;` 或 `display: inline-flex;` 声明的元素，其所有直接子元素都将成为 flex 项目。
- **项目（Flex Items）**：在 flex 容器内的直接子元素。

### 容器属性

- **display**

  - `display: flex;`：将元素设为块级 flex 容器。
  - `display: inline-flex;`：将元素设为内联级 flex 容器。

- **flex-direction**

  - 定义主轴的方向，决定项目的排列方向：
    - `row`（默认）：水平方向，从左到右。
    - `row-reverse`：水平方向，从右到左。
    - `column`：垂直方向，从上到下。
    - `column-reverse`：垂直方向，从下到上。

- **flex-wrap**

  - 定义当项目总尺寸超出容器时是否换行：
    - `nowrap`（默认）：不换行。
    - `wrap`：换行，第一行在上方。
    - `wrap-reverse`：换行，第一行在下方。

- **justify-content**

  - 定义在主轴上的对齐方式：
    - `flex-start`（默认）：左对齐或起点对齐。
    - `flex-end`：右对齐或结束对齐。
    - `center`：居中对齐。
    - `space-between`：两端对齐，项目之间的间隔相等。
    - `space-around`：项目两侧的间隔相等。

- **align-items**

  - 定义在交叉轴（与主轴垂直方向）的对齐方式：
    - `stretch`（默认）：拉伸填充容器。
    - `flex-start`：交叉轴起点对齐。
    - `flex-end`：交叉轴结束对齐。
    - `center`：居中对齐。
    - `baseline`：以项目的第一行文字基线对齐。

- **align-content**
  - 定义多行对齐方式（当项目换行时）：
    - `stretch`（默认）：各行拉伸到占满容器。
    - `flex-start`：各行顶端对齐。
    - `flex-end`：各行底端对齐。
    - `center`：居中对齐。
    - `space-between`：行与行之间间隔相等。
    - `space-around`：行之间的间隔相等。

### 项目属性

- **flex-grow**

  - 定义项目的放大比例。如果空间充裕，flex 项目可以根据它的增长因子扩展。

- **flex-shrink**

  - 定义项目的缩小比例。当空间不足时，flex 项目可以根据它的缩小因子收缩。

- **flex-basis**

  - 定义在分配多余空间之前，项目占据的主轴空间。可以设置为固定值（例如 `200px`）也可以设置为 `auto`。

- **flex**

  - flex 属性是 flex-grow、flex-shrink 和 flex-basis 的简写形式。常用写法如：`flex: 1;` 表示 `flex-grow: 1; flex-shrink: 1; flex-basis: 0%;`。

- **align-self**
  - 允许单个项目有不同于其他项目的对齐方式，可以覆盖容器的 `align-items` 属性。
  - 取值：`auto`（默认）、`flex-start`、`flex-end`、`center`、`baseline`、`stretch`。

## Grid

![grid布局](grid.png)

CSS Grid 布局是一种基于网格的二维布局系统，用于创建复杂和响应式的网页布局。与 Flexbox 不同，CSS Grid 能够同时处理行和列，使其成为构建整体页面布局的强大工具。

- **容器（Grid Container）：** 使用 `display: grid;` 或 `display: inline-grid;` 声明的元素。其所有直接子元素将自动成为网格项目，并根据指定的网格定义进行排列。
- **项目（Grid Items）：** Grid 容器内的直接子元素，每个项目在网格中按照指定的位置排布。
- **网格线（Grid Lines）：** 网格中的水平和垂直分隔线，用来分割行和列。
- **网格单元（Grid Cell）：** 两条相邻的网格线之间形成的区域，类似于表格中的单元格。
- **网格区域（Grid Area）：** 由一个或多个网格单元组成的区域，可为网格项目命名以便布局控制。

### 容器属性

- **display:** 声明一个元素为 Grid 容器。

  - `display: grid;` 用于块级 Grid 容器。
  - `display: inline-grid;` 用于内联级 Grid 容器。

- **grid-template-columns:** 定义网格的列。可以使用固定长度、百分比、fr 单位或者 `repeat()` 函数来指定列的布局。

  - 示例：
    ```css
    grid-template-columns: 200px 1fr 2fr;
    ```
  - 或者：
    ```css
    grid-template-columns: repeat(3, 1fr);
    ```

- **grid-template-rows:** 定义网格的行，写法和 `grid-template-columns` 类似。

  - 示例：
    ```css
    grid-template-rows: 100px auto;
    ```

- **gap (或 grid-gap):** 定义网格行和列之间的间隙。

  - 示例：
    ```css
    gap: 10px;
    ```

- **grid-template-areas:** 定义命名网格区域，使布局更加具语义化。

  - 示例：
    ```css
    grid-template-areas:
      "header header"
      "sidebar content"
      "footer footer";
    ```

- **justify-items & align-items:** 分别设置网格项目在各自网格单元内的水平和垂直对齐方式。

  - 示例：
    ```css
    justify-items: center; /* 水平居中 */
    align-items: center; /* 垂直居中 */
    ```

- **justify-content & align-content:** 设置整个网格在容器内的对齐方式，适用于有多余空间的情况。
  - 示例：
    ```css
    justify-content: space-around;
    align-content: center;
    ```

### 项目属性

- **grid-column & grid-row:** 定义项目在网格中所占据的列和行范围。可以使用网格线编号或命名区域。

  - 示例：
    ```css
    grid-column: 1 / 3;
    grid-row: 2 / 4;
    ```

- **grid-area:** 用于将项目放置在命名的网格区域内，通常与 `grid-template-areas` 配合使用。
  - 示例：
    ```css
    grid-area: header;
    ```
