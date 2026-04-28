# javascript高级

## 类型与内存基础

### typeof 与 instanceof

#### typeof

`typeof` 是一个一元操作符，用于返回变量的数据类型，返回字符串。

**语法：**

```js
typeof xxx;
```

**返回值对照表：**

| 类型 | Undefined   | Null     | Boolean   | Number   | BigInt   | String   | Symbol   | Function   | Object   |
| ---- | ----------- | -------- | --------- | -------- | -------- | -------- | -------- | ---------- | -------- |
| 结果 | "undefined" | "object" | "boolean" | "number" | "bigint" | "string" | "symbol" | "function" | "object" |

::: warning ⚠️ 注意

- `typeof null` 返回 `"object"` 是一个著名的 JavaScript 历史遗留问题，并非设计意图
- `typeof` 是唯一可以用来检测未声明变量的操作符（不会抛出 ReferenceError）

```js
// 检测未声明的变量不会报错
typeof undeclaredVariable; // "undefined"
```

:::

#### instanceof

`instanceof` 用于检测构造函数的 prototype 属性是否出现在对象的原型链中，返回布尔值。

**语法：**

```js
object instanceof constructor;
```

**工作原理：**

`instanceof` 会检查对象的原型链中是否存在构造函数的 prototype 属性。

```js
function Person(name) {
  this.name = name;
}

const person = new Person("Tom");

person instanceof Person; // true
person instanceof Object; // true （Object 是所有对象的顶层原型）
```

#### 区别

| 特性      | typeof               | instanceof |
| --------- | -------------------- | ---------- |
| 适用对象  | 原始类型和函数       | 对象实例   |
| 返回值    | 字符串               | 布尔值     |
| 数组检测  | 返回 "object"        | 返回 true  |
| null 检测 | 返回 "object"（bug） | 不适用     |
| 原型链    | 不涉及               | 涉及原型链 |

**选择建议：**

- 检测基本类型时使用 `typeof`
- 检测引用类型实例时使用 `instanceof`
- 检测数组时可以使用 `Array.isArray()`（推荐）或 `instanceof`
- 检测 null 时使用 `=== null`

```js
// 推荐的类型检测方式
typeof "str" === "string";
typeof 123 === "number";
typeof true === "boolean";
typeof undefined === "undefined";
Array.isArray([]); // true
value === null;
```

### undefined 与 null

`undefined` 表示变量已声明但未初始化，或者访问对象不存在的属性，以及函数没有返回值时默认返回的值。

`null` 表示一个空对象指针，是 JavaScript 中的关键字，用于表示"刻意设置为空"或"无值"。

| 特性       | undefined    | null           |
| ---------- | ------------ | -------------- |
| 类型       | "undefined"  | "object"       |
| 语义       | 未定义       | 已定义但为空   |
| 用途       | 系统自动产生 | 开发者主动设置 |
| 转换为数字 | NaN          | 0              |

```js
// 类型转换
Number(undefined); // NaN
Number(null); // 0

// 相等性比较
undefined == null; // true （宽松相等）
undefined === null; // false （严格相等）
```

### 堆栈内存

| 栈内存                 | 堆内存                       |
| ---------------------- | ---------------------------- |
| 存储基础数据类型       | 存储引用数据类型             |
| 按值访问               | 按引用访问                   |
| 存储的值大小固定       | 存储的值大小不定，可动态调整 |
| 由系统自动分配内存空间 | 由代码进行指定分配           |
| 空间小，运行效率高     | 空间大，运行效率相对较低     |
| 先进后出，后进先出     | 无序存储，可根据引用直接获取 |

## 作用域、上下文与闭包

### 执行上下文与执行上下文栈

#### 执行上下文

执行上下文是 JavaScript 代码执行时的环境抽象概念，当 JavaScript 代码运行时，会创建一个执行上下文来管理代码的执行。每个执行上下文包含了代码运行时所需的信息：

**执行上下文的组成：**

- **变量环境**：存储 `var` 声明的变量、函数声明
- **词法环境**：存储 `let`、`const` 声明的变量
- **this 绑定**：确定 `this` 的指向

**执行上下文的类型：**

1. **全局执行上下文** - 代码首次执行时创建，整个脚本只有一个
   - 创建一个全局对象（如 `window` 或 `global`）
   - 将 `this` 绑定到全局对象
2. **函数执行上下文** - 每当调用一个函数时创建
   - 每次函数调用都会创建新的执行上下文
   - 函数执行完毕上下文被销毁

```js
// 全局执行上下文
console.log(this); // 浏览器中指向 window

function outer() {
  // outer 函数的执行上下文
  function inner() {
    // inner 函数的执行上下文
  }
  inner();
}
outer();
```

#### 执行上下文栈

执行上下文栈，也称为调用栈，是一种 LIFO（后进先出）的数据结构，用于管理多个执行上下文的切换。

**工作原理：**

1. 当代码开始执行，创建全局执行上下文并压入栈底
2. 每当调用一个函数，创建新的函数执行上下文并压入栈顶
3. 函数执行完毕后，其执行上下文从栈顶弹出
4. 最后只剩下全局执行上下文

**示例：**

```js
function a() {
  console.log("a start");
  b();
  console.log("a end");
}

function b() {
  console.log("b start");
  c();
  console.log("b end");
}

function c() {
  console.log("c");
}

a();

// 打印结果：a start、b start、c、b end、a end
```

**调用栈的变化过程：**

```
1. 全局上下文入栈
2. a() 调用，a 上下文入栈
3. b() 调用，b 上下文入栈
4. c() 调用，c 上下文入栈
5. c() 执行完毕，c 上下文出栈
6. b() 执行完毕，b 上下文出栈
7. a() 执行完毕，a 上下文出栈
8. 全局上下文保留（页面关闭时出栈）
```

#### 执行上下文创建过程

每个执行上下文的创建分为两个阶段：

**1. 创建阶段**

- 创建变量对象（VO/AO）
- 建立作用域链
- 确定 this 指向

**2. 执行阶段**

- 变量赋值
- 函数引用
- 执行代码

```js
function executionContextExample() {
  console.log(a); // undefined （创建阶段已声明，值为 undefined）
  var a = 10;
  console.log(a); // 10 （执行阶段已完成赋值）
}
executionContextExample();
```

::: info 栈溢出

当调用栈中的执行上下文数量超过浏览器限制时，会发生栈溢出错误。

```js
function recursive() {
  recursive();
}
recursive(); // RangeError: Maximum call stack size exceeded
```

:::

### 作用域与作用域链

#### 作用域

作用域是指变量、函数和对象的可访问范围或生效区域。JavaScript 中的作用域决定了代码段中变量的可见性和生命周期。

**作用域的类型：**

1. **全局作用域** - 在代码任何位置都能访问
   - 在函数外部定义的变量
   - 未使用 var/let/const 直接赋值的变量（严格模式下报错）

2. **函数作用域** - 仅在函数内部可访问
   - 在函数内部用 var 声明的变量
   - 函数的参数

3. **块级作用域** - 仅在代码块内部可访问（let 和 const）
   - if、for、while 等语句块
   - 花括号 `{ }` 包裹的区域

::: info var、let、const 的作用域区别

```js
// var - 函数作用域，存在变量提升
function varTest() {
  if (true) {
    var varVariable = "var";
  }
  console.log(varVariable); // 'var' - 可以访问
}

// let - 块级作用域，不存在变量提升（暂时性死区）
function letTest() {
  if (true) {
    let letVariable = "let";
    console.log(letVariable); // 'let'
  }
  // console.log(letVariable);  // ReferenceError
}

// const - 块级作用域，必须初始化
function constTest() {
  const PI = 3.14159;
  // const constVariable;  // SyntaxError: 必须初始化
}
```

:::

#### 作用域链

作用域链是由多个执行上下文的变量环境组成的链表，用于解析变量。当访问一个变量时，JavaScript 会沿着作用域链从内到外依次查找，直到找到为止。

**工作原理：**

1. 当代码在某个作用域中执行时，会创建一个作用域链
2. 作用域链包含当前作用域的所有外层作用域的引用
3. 访问变量时，从当前作用域开始查找，逐层向外查找
4. 找到第一个匹配的变量后就停止查找

```js
const globalVar = "全局变量";
function outer() {
  const outerVar = "外层变量";
  function middle() {
    const middleVar = "中间层变量";
    function inner() {
      const innerVar = "内层变量";
      // 变量查找顺序：innerVar -> middleVar -> outerVar -> globalVar
      console.log(innerVar); // '内层变量'
      console.log(middleVar); // '中间层变量'
      console.log(outerVar); // '外层变量'
      console.log(globalVar); // '全局变量'
    }
    inner();
  }
  middle();
}
outer();
```

#### 作用域与执行上下文的区别

| 特性     | 作用域               | 执行上下文           |
| -------- | -------------------- | -------------------- |
| 定义时期 | 代码编写时确定       | 代码运行时创建       |
| 数量     | 静态（代码结构决定） | 动态（函数调用决定） |
| 生命周期 | 变量声明时创建       | 函数调用时创建       |
| 变化     | 不变                 | 随函数调用变化       |

### 闭包

## 面向对象编程 (OOP)

### 原型与原型链

### 对象创建模式

### 继承模式

## JavaScript 运行时与浏览器

### 进程与线程

### 浏览器内核

### js为什么是单线程的

### 浏览器的事件循环
