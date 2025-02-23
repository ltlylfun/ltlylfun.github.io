---
title: 关于TypeScript的笔记
description: 记录一些TypeScript笔记
date: 2025-02-18T20:20:09+08:00
comments: true
keywords: [TypeScript]
categories:
  - TypeScript-Notes
tags:
  - TypeScript
---

## 前言

在前端开发中，TypeScript 是一门非常重要的语言，它为 JavaScript 引入了静态类型检查，提升了代码的可靠性和可维护性，对于构建健壮的应用程序至关重要。

希望这些笔记不仅能帮助自己梳理知识体系，也能为其他使用 TypeScript 的开发者提供参考和启发。

PS 不定时更新，欢迎讨论区留言。

## 类型系统

### 基本类型

在 TypeScript 中，类型系统包括许多基本类型，用于描述数据的不同形态。下面列出了常见的基本类型及其示例代码：

1. boolean（布尔类型）
   用于表示逻辑值 `true` 或 `false`。

```typescript
let isActive: boolean = true;
```

2. string（字符串类型）
   用于表示文本数据，可使用单引号、双引号或模板字符串。

```typescript
let greeting: string = "Hello, TypeScript!";
```

3. number（数字类型）
   用于表示整数、浮点数、十六进制、二进制和八进制字面量。

```typescript
let age: number = 30;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

4. bigint（大整数类型）
   用于表示大于 `2^53 - 1` 的整数。字面量后缀 `n` 用于标识 bigint 类型。

```typescript
let bigNumber: bigint = 123456789012345678901234567890n;
```

5. symbol（符号类型）
   用于创建独一无二的标识符，常用于对象属性的键。

```typescript
let uniqueKey: symbol = Symbol("uniqueKey");
```

6. object（对象类型）
   表示除基本类型之外的所有类型。用于描述具有属性的复合数据结构。

```typescript
let person: object = { name: "Alice", age: 25 };
```

7. undefined（未定义类型）
   表示未赋值状态，只有一个唯一值 `undefined`。

```typescript
let nothing: undefined = undefined;
```

8. null（空值类型）
   表示空或不存在的值，只有一个唯一值 `null`。

```typescript
let empty: null = null;
```

### 联合类型

在 TypeScript 中，联合类型（Union Types）允许一个变量可以是多个类型中的一个。使用 `|` 来分隔各个可能的类型，从而定义一个变量可能接受的所有类型。联合类型使得代码更具灵活性，同时保持静态类型检查的优势。

例如，下面这个变量可以是数字或者字符串：

```typescript
let value: number | string;
value = 42;
value = "Hello, TypeScript!";

// 如果赋值为其他类型，例如 boolean，则会报错：
// value = true; // Error: Type 'boolean' is not assignable to type 'number | string'.
```

### 交叉类型

在 TypeScript 中，交叉类型（Intersection Types）允许你将多个类型合并为一个类型。当一个变量同时满足多个独立类型的要求时，就可以使用交叉类型。这种类型表示法使用 `&` 来连接各个类型，从而创建出一个新的类型，该类型包含所有合并类型的属性和方法。

假设我们有两个接口，分别描述了不同的属性：

```typescript
interface Person {
  name: string;
  age: number;
}

interface Employee {
  employeeId: number;
  department: string;
}
```

我们可以使用交叉类型将这两个接口合并，创建一个既是 `Person` 又是 `Employee` 的类型：

```typescript
type Manager = Person & Employee;

const manager: Manager = {
  name: "Alice",
  age: 30,
  employeeId: 12345,
  department: "Sales",
};
```

在这个例子中，`Manager` 类型必须同时具有 `Person` 和 `Employee` 的所有属性。

### type

`type`命令用来定义一个类型的别名。

```
type Age = number;

let age:Age = 55;
```

上面示例中，`type`命令为 number 类型定义了一个别名 Age。这样就能像使用 number 一样，使用 Age 作为类型。

别名可以让类型的名字变得更有意义，也能增加代码的可读性，还可以使复杂类型用起来更方便，便于以后修改变量的类型。

别名不允许重名。

```
type Color = 'red';
type Color = 'blue'; // 报错
```

上面示例中，同一个别名 Color 声明了两次，就报错了。

别名的作用域是块级作用域。这意味着，代码块内部定义的别名，影响不到外部。

### TypeScript `typeof` 操作符

在 TypeScript 中，`typeof` 操作符有两种主要用途：

- 获取一个表达式在运行时的类型（类似于 JavaScript 中的 `typeof`）。
- 在类型层面上查询变量或表达式的类型，以便在其他地方重用该类型（类型查询）。

1. 运行时使用 `typeof`

在运行时，`typeof` 操作符返回一个字符串，描述了一个值的类型。这与 JavaScript 完全一致。常见的返回值包括 `"string"`, `"number"`, `"boolean"`, `"undefined"`, `"object"`, 和 `"function"`。

例如：

```typescript name=RuntimeTypeof.ts
const someValue = 42;

if (typeof someValue === "number") {
  console.log("someValue 是一个数字");
}
```

2. 类型层面上的 `typeof`（类型查询）

TypeScript 允许你在类型层面上使用 `typeof` 来获取一个变量或对象的类型，并将其用于类型声明中。这种用法称为“类型查询”，它可以帮助你重用已有的类型，而无需重复编写接口或类型定义。

例如：

```typescript name=TypeQueryExample.ts
const person = {
  name: "Alice",
  age: 30,
};

// 使用 typeof person 获取 person 的类型，并创建一个类型别名
type PersonType = typeof person;

const anotherPerson: PersonType = {
  name: "Bob",
  age: 25,
};
```

在这个例子中，`typeof person` 获取了 `person` 变量的类型，并创建了名为 `PersonType` 的类型，这样可以避免手动定义重复的类型结构。

### 块级类型声明

TypeScript 支持块级类型声明，即类型可以声明在代码块（用大括号表示）里面，并且只在当前代码块有效。

```
if (true) {
  type T = number;
  let v:T = 5;
} else {
  type T = string;
  let v:T = 'hello';
}
```

上面示例中，存在两个代码块，其中分别有一个类型 T 的声明。这两个声明都只在自己的代码块内部有效，在代码块外部无效。

## 数组

### 使用方括号语法

最常见的方式是直接在类型后面加上方括号 `[]` 表示数组类型。

```typescript name=example1.ts
let numbers: number[] = [1, 2, 3, 4, 5];
let fruits: string[] = ["apple", "banana", "cherry"];
```

### 使用泛型语法

TypeScript 提供了泛型数组语法 `Array<元素类型>`，其效果与方括号语法完全相同。

```typescript name=example2.ts
let numbers: Array<number> = [1, 2, 3, 4, 5];
let fruits: Array<string> = ["apple", "banana", "cherry"];
```

### 多维数组

多维数组是数组的数组。例如，二维数组可以表示为数组中的每个元素都是一个数组。

```typescript name=example4.ts
let matrix: number[][] = [
  [1, 2],
  [3, 4],
  [5, 6],
];

console.log(matrix);
// 输出:
// [
//   [1, 2],
//   [3, 4],
//   [5, 6]
// ]
```

### 元组

在 TypeScript 中，元组（Tuple）是一种特殊类型的数组，它允许你在同一个数组中存储多个不同类型的值，同时要求元素的个数和顺序固定。使用元组，能够更明确地描述混合类型数据的结构。

- 定义元组

你可以使用带有固定数量和类型的元素来定义元组。例如，下面我们定义了一个包含数字和字符串的元组：

```typescript name=ExampleTuple.ts
let user: [number, string] = [1, "Alice"];

// 正确: 按照定义提供了 number 和 string 类型的值
console.log(user);

// 错误示例: 交换顺序会导致类型错误
// let wrongUser: [number, string] = ["Alice", 1];
```

- 元组的解构赋值

元组支持解构赋值，你可以直接把元组的每个元素赋值给单独的变量：

```typescript name=DestructureTuple.ts
let user: [number, string] = [1, "Alice"];
let [userId, userName] = user;

console.log(userId); // 输出: 1
console.log(userName); // 输出: Alice
```

- 可选的元组元素

TypeScript 允许在元组中定义可选元素，这些可选元素必须放在元组的末尾。例如：

```typescript name=OptionalTuple.ts
let response: [number, string?] = [200];

console.log(response); // 输出: [200]

// 此外，也可以提供所有元素
let fullResponse: [number, string?] = [200, "OK"];
console.log(fullResponse); // 输出: [200, "OK"]
```

## 函数类型

### 函数声明

通过函数声明，你可以直接在函数签名中定义参数和返回值的类型。

```typescript name=functionDeclaration.ts
function greet(name: string): string {
  return `Hello, ${name}!`;
}

console.log(greet("Alice"));
```

### 函数表达式

你也可以使用函数表达式的方式定义函数，同时为变量指定函数类型。

```typescript name=functionExpression.ts
const add: (a: number, b: number) => number = function (a, b) {
  return a + b;
};

console.log(add(2, 3));
```

### 可选参数和默认参数

TypeScript 允许在函数中使用可选参数和默认参数，使得函数调用更灵活。

```typescript name=optionalDefaultParameters.ts
// 可选参数 lastName 使用 ? 标记
function buildName(firstName: string, lastName?: string): string {
  return lastName ? `${firstName} ${lastName}` : firstName;
}

console.log(buildName("John"));
console.log(buildName("John", "Doe"));

// 默认参数 b 默认值为 2
function multiply(a: number, b: number = 2): number {
  return a * b;
}

console.log(multiply(5)); // 输出: 10
console.log(multiply(5, 3)); // 输出: 15
```

### 剩余参数

当函数需要接受任意数量的参数时，可以使用剩余参数（rest parameters）。

```typescript name=restParameters.ts
function joinStrings(separator: string, ...strings: string[]): string {
  return strings.join(separator);
}

console.log(joinStrings(", ", "apple", "banana", "cherry")); // 输出: apple, banana, cherry
```

### 函数类型作为参数和返回值

TypeScript 可以定义函数类型，将其用作参数类型或返回值类型，从而实现高阶函数。

```typescript name=higherOrderFunction.ts
// 定义一个函数类型 Operation
type Operation = (a: number, b: number) => number;

// 实现加法函数
const add: Operation = (a, b) => a + b;

// 高阶函数，接收一个 Operation 类型的函数作为参数
function calculate(a: number, b: number, op: Operation): number {
  return op(a, b);
}

console.log(calculate(4, 5, add)); // 输出: 9
```

### 函数重载

函数重载允许同一个函数根据不同的参数类型或数量提供多种签名，从而实现多态。

```typescript name=overloadFunctions.ts
// 函数重载的声明
function combine(x: string, y: string): string;
function combine(x: number, y: number): number;

// 实现函数重载的函数体
function combine(x: any, y: any): any {
  return x + y;
}

console.log(combine("Hello, ", "World!")); // 输出: Hello, World!
console.log(combine(10, 20)); // 输出: 30
```

## 对象

### 使用接口定义对象类型

```typescript name=UserInterface.ts
interface User {
  id: number;
  name: string;
  email: string;
  // 可选属性, 如果没有提供不会报错
  age?: number;
  // 只读属性, 一旦赋值后就不能修改
  readonly createdAt: Date;
}

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  createdAt: new Date(),
};

// 下面的修改将发生错误，因为 createdAt 是只读属性
// user.createdAt = new Date();
console.log(user);
```

### 使用类型别名定义对象类型

```typescript name=UserTypeAlias.ts
type User = {
  id: number;
  name: string;
  email: string;
  age?: number; // 可选属性
  readonly createdAt: Date; // 只读属性
};

const user: User = {
  id: 2,
  name: "Bob",
  email: "bob@example.com",
  createdAt: new Date(),
};

console.log(user);
```

### 定义对象上的方法

除了描述属性，TypeScript 的对象类型同样可以描述方法。下面是一个例子：

```typescript name=UserWithMethods.ts
interface User {
  id: number;
  name: string;
  email: string;
  greet(): string;
}

const user: User = {
  id: 3,
  name: "Charlie",
  email: "charlie@example.com",
  greet() {
    return `Hello, my name is ${this.name}`;
  },
};

console.log(user.greet());
```

### 对象字面量的类型检查

在 TypeScript 中，当你直接使用对象字面量创建对象时，编译器会对属性的存在性和类型进行严格检查。这帮助我们发现拼写错误或遗漏的属性：

```typescript name=LiteralObjects.ts
interface Config {
  port: number;
  host: string;
}

const config: Config = {
  port: 8080,
  host: "localhost",
  // 如果加上多余属性，例如 protocol: "http"，编译器将报错
};

console.log(`Server running at http://${config.host}:${config.port}`);
```

### 使用索引签名定义动态属性的对象

当你需要定义一个包含动态属性名的对象时，可以使用索引签名。如下例子展示了如何定义一个可以包含任意字符串键及其对应值的对象：

```typescript name=Dictionary.ts
interface Dictionary {
  [key: string]: string;
}

const translations: Dictionary = {
  hello: "你好",
  goodbye: "再见",
};

console.log(translations.hello);
```

## interface

### 定义基本接口

下面的示例展示了如何定义一个接口 `User`，描述用户对象的属性：

```typescript name=UserInterface.ts
interface User {
  id: number;
  name: string;
  age: number;
  email?: string; // 可选属性，用 ? 标记
}

const user: User = {
  id: 1,
  name: "Alice",
  age: 30,
  email: "alice@example.com",
};

console.log(user);
```

### 接口中的只读属性

接口可以定义只读属性，一旦对象被创建后，这些属性就不能被修改，从而增强数据的安全性。

```typescript name=ReadOnlyUser.ts
interface ReadOnlyUser {
  readonly id: number;
  name: string;
}

const user: ReadOnlyUser = {
  id: 100,
  name: "Bob",
};

// 下面这行代码会报错，因为 id 是只读属性
// user.id = 101;
```

### 接口中的方法

可以在接口中定义方法，这样实现接口的对象必须提供方法的具体实现。

```typescript name=UserWithMethods.ts
interface User {
  id: number;
  name: string;
  greet(message: string): string;
}

const user: User = {
  id: 2,
  name: "Charlie",
  greet(message: string) {
    return `Hello ${this.name}, ${message}`;
  },
};

console.log(user.greet("welcome!"));
```

### 接口的嵌套和继承

接口不仅可以描述简单对象，还可以通过嵌套和继承来构建更复杂的类型结构。

```typescript name=ExtendInterface.ts
interface Person {
  name: string;
  age: number;
}

interface Employee extends Person {
  employeeId: number;
  department: string;
}

const employee: Employee = {
  name: "David",
  age: 28,
  employeeId: 12345,
  department: "Engineering",
};

console.log(employee);
```

### 使用接口描述函数类型

接口同样可以用来描述函数类型，这样可以明确函数的参数和返回值类型，提高代码的可读性和稳定性。

```typescript name=FunctionInterface.ts
interface Comparator {
  (a: number, b: number): number;
}

const compare: Comparator = (x, y) => {
  if (x < y) return -1;
  if (x > y) return 1;
  return 0;
};

console.log(compare(3, 5));
```
