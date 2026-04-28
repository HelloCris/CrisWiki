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

### 作用域与作用域链

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
