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

#### 闭包概述

当一个嵌套的内部(子)函数引用了嵌套的外部(父)函数的变量(函数)时, 就产生了闭包

**简单理解：**

- 闭包 = 函数 + 函数能够访问的外部变量
- 内部函数可以访问外部函数的局部变量

```js
function outer() {
  const name = "Tom";
  function inner() {
    console.log(name); // inner 可以访问 outer 的变量
  }
  return inner;
}
const fn = outer();
fn(); // 输出 'Tom'
```

::: info 闭包的本质

闭包的实现依赖于 JavaScript 的作用域链机制：

1. 函数在创建时会保存当前的作用域链
2. 当函数执行时，会创建新的执行上下文
3. 执行上下文的作用域链 = 当前执行上下文 + 外部函数的作用域链
4. 即使外部函数已经执行完毕，其变量对象仍然被内部函数引用，不会被垃圾回收

```js
function outer() {
  const count = 0; // 外部变量
  return function () {
    count++; // 每次调用都会修改外部变量
    console.log(count);
  };
}
const fn = outer();
fn(); // 1
fn(); // 2
fn(); // 3
```

:::

::: info 闭包的优势与劣势

**优势：**

- 实现私有变量和数据封装
- 延续变量生命周期
- 实现函数式编程特性

**劣势：**

- 占用内存时间长（外部函数变量无法被垃圾回收）
- 增加内存消耗
- 使用不当可能导致内存泄漏

:::

#### 闭包的问题

**内存泄漏：**

```js
function badPractice() {
  const largeData = new Array(1000000);
  return function () {
    console.log(largeData.length);
  };
}
const fn = badPractice();
// largeData 不会被回收，因为被闭包引用
fn = null; // 手动解除引用
```

**this 指向问题：**

```js
const obj = {
  name: "obj",
  getName() {
    return function () {
      return this.name; // this 指向 global，不是 obj
    };
  },
};
console.log(obj.getName()()); // undefined
// 修正方法
const obj2 = {
  name: "obj",
  getName() {
    const self = this;
    return function () {
      return self.name;
    };
  },
};
console.log(obj2.getName()()); // 'obj'
```

#### 经典题目

```js
// 题目一：循环中的闭包
for (var i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i);
  }, 0);
}
// 输出：3 3 3

// 题目二：解决方案
for (var i = 0; i < 3; i++) {
  (function (j) {
    setTimeout(function () {
      console.log(j);
    }, 0);
  })(i);
}
// 输出：0 1 2

// 题目三：let 解决
for (let i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i);
  }, 0);
}
// 输出：0 1 2
```

## 面向对象编程 (OOP)

### 原型与原型链

#### 原型基本概念

在 JavaScript 中，每个对象都有一个隐式属性 `__proto__`，指向它的原型对象。原型对象也是一个普通对象，同样拥有自己的原型，这样就形成了一条原型链。

- **prototype**：函数特有的属性，指向函数的原型对象
- **\_\_proto\_\_**：对象隐式原型属性，指向创建该对象的构造函数的 prototype
- **constructor**：原型对象的属性，指向构造函数本身

```js
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function () {
  console.log(`Hello, I'm ${this.name}`);
};

const person = new Person("张三");

console.log(person.__proto__ === Person.prototype); // true
console.log(Person.prototype.constructor === Person); // true
console.log(person.constructor === Person); // true
```

#### 原型链

当访问一个对象的属性时，如果对象本身没有该属性，JavaScript 会沿着原型链向上查找，直到找到或者到达 Object.prototype 为止。

**示意图：**

```
person 实例
    │
    │ __proto__
    ▼
Person.prototype
    │
    │ __proto__
    ▼
Object.prototype
    │
    │ __proto__
    ▼
null
```

::: info 原型链特性

- **属性查找**：先找自身属性，再找原型属性，原型属性优先级低于自身属性
- **属性遮蔽**：自身属性会覆盖原型上的同名属性
- **方法继承**：通过原型链，可以实现方法的复用和共享
- **原型链终点**：`Object.prototype.__proto__` 为 `null`

:::

`Object.create()` 方法可以显式指定对象的原型。  
`instanceof` 运算符可以检测对象是否是某个构造函数的实例。

### 对象创建模式

**方式 1: Object 构造函数模式**

- 套路: 先创建空 Object 对象, 再动态添加属性/方法
- 适用场景: 起始时不确定对象内部数据
- 问题: 语句太多

示例:

```js
// 先创建空 Object 对象
var p = new Object();
p = {}; //此时内部数据是不确定的
// 再动态添加属性/方法
p.name = "Tom";
p.age = 12;
p.setName = function (name) {
  this.name = name;
};
```

**方式 2: 对象字面量模式**

- 套路: 使用{}创建对象, 同时指定属性/方法
- 适用场景: 起始时对象内部数据是确定的
- 问题: 如果创建多个对象, 有重复代码

示例:

```js
var p = {
  name: "Tom",
  age: 12,
  setName: function (name) {
    this.name = name;
  },
};
```

**方式 3: 工厂模式**

- 套路: 通过工厂函数动态创建对象并返回
- 适用场景: 需要创建多个对象
- 问题: 对象没有一个具体的类型, 都是 Object 类型

示例:

```js
function createPerson(name, age) {
  //返回一个对象的函数===>工厂函数
  var obj = {
    name: name,
    age: age,
    setName: function (name) {
      this.name = name;
    },
  };
  return obj;
}
// 创建 2 个人
var p1 = createPerson("Tom", 12);
var p2 = createPerson("Bob", 13);
```

**方式 4: 自定义构造函数模式**

- 套路: 自定义构造函数, 通过 new 创建对象
- 适用场景: 需要创建多个类型确定的对象
- 问题: 每个对象都有相同的数据, 浪费内存

示例:

```js
//定义类型
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.setName = function (name) {
    this.name = name;
  };
}
var p1 = new Person("Tom", 12);
```

**方式 5: 构造函数+原型的组合模式**

- 套路: 自定义构造函数, 属性在函数中初始化, 方法添加到原型上
- 适用场景: 需要创建多个类型确定的对象

示例:

```js
function Person(name, age) {
  //在构造函数中只初始化一般函数
  this.name = name;
  this.age = age;
}
Person.prototype.setName = function (name) {
  this.name = name;
};
```

### 继承模式

#### 原型链继承

通过将子类的原型指向父类的实例来实现继承。

```js
function Animal(name) {
  this.name = name;
}

Animal.prototype.getName = function () {
  return this.name;
};

function Dog(name, breed) {
  this.breed = breed;
}

Dog.prototype = new Animal();
Dog.prototype.constructor = Dog;
Dog.prototype.bark = function () {
  console.log("汪汪汪");
};

const dog = new Dog("旺财", "柯基");
console.log(dog.name); // '旺财' - 继承自 Animal
console.log(dog.breed); // '柯基' - 自身属性
console.log(dog.getName()); // '旺财' - 继承的方法
dog.bark(); // '汪汪汪'
```

**缺点**：

- 父类属性如果是引用类型，会被所有子类实例共享
- 无法向父类构造函数传参

#### 构造函数继承（经典继承）

在子类构造函数内部调用父类构造函数。

```js
function Animal(name) {
  this.name = name;
  this.colors = ["白色", "黑色"];
}

Animal.prototype.getName = function () {
  return this.name;
};

function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}

const dog = new Dog("旺财", "柯基");

console.log(dog.name); // '旺财'
console.log(dog.breed); // '柯基'
console.log(dog.colors); // ['白色', '黑色']

const dog1 = new Dog("旺财1", "柯基");
const dog2 = new Dog("旺财2", "金毛");

dog1.colors.push("黄色");

console.log(dog1.colors); // ['白色', '黑色', '黄色']
console.log(dog2.colors); // ['白色', '黑色'] - 不受影响
```

**缺点**：

- 方法必须在构造函数内定义，无法复用
- 父类原型上的方法子类无法访问

#### 组合继承

结合原型链继承和构造函数继承，是最常用的继承方式。

```js
function Animal(name) {
  this.name = name;
  this.colors = ["白色", "黑色"];
}

Animal.prototype.getName = function () {
  return this.name;
};

function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}

Dog.prototype = new Animal();
Dog.prototype.constructor = Dog;
Dog.prototype.bark = function () {
  console.log("汪汪汪");
};

const dog = new Dog("旺财", "柯基");

console.log(dog.name); // '旺财'
console.log(dog.breed); // '柯基'
console.log(dog.getName()); // '旺财'
dog.bark(); // '汪汪汪'

const dog1 = new Dog("旺财1", "柯基");
const dog2 = new Dog("旺财2", "金毛");

dog1.colors.push("黄色");

console.log(dog1.colors); // ['白色', '黑色', '黄色']
console.log(dog2.colors); // ['白色', '黑色']
```

**缺点**：调用了两次父类构造函数（`Animal.call()` 和 `new Animal()`）

#### 原型式继承

使用 `Object.create()` 实现继承。

```js
const animal = {
  name: "动物",
  getName: function () {
    return this.name;
  },
};

const dog = Object.create(animal);
dog.name = "旺财";
dog.bark = function () {
  console.log("汪汪汪");
};

console.log(dog.getName()); // '旺财'
console.log(dog.__proto__ === animal); // true
```

#### 寄生式继承

在原型式继承的基础上，增强创建的对象。

```js
function createDog(name, breed) {
  const dog = Object.create(Animal.prototype);
  dog.name = name;
  dog.breed = breed;
  dog.bark = function () {
    console.log("汪汪汪");
  };
  return dog;
}

const dog = createDog("旺财", "柯基");
dog.bark();
```

**缺点**：方法在函数内定义，无法复用

#### 寄生组合式继承

目前最理想的继承方式，结合寄生式继承和组合继承。

```js
function inheritPrototype(SubType, SuperType) {
  const prototype = Object.create(SuperType.prototype);
  prototype.constructor = SubType;
  SubType.prototype = prototype;
}

function Animal(name) {
  this.name = name;
  this.colors = ["白色", "黑色"];
}

Animal.prototype.getName = function () {
  return this.name;
};

function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}

inheritPrototype(Dog, Animal);

Dog.prototype.bark = function () {
  console.log("汪汪汪");
};

const dog = new Dog("旺财", "柯基");

console.log(dog.name); // '旺财'
console.log(dog.breed); // '柯基'
console.log(dog.getName()); // '旺财'
dog.bark(); // '汪汪汪'
```

#### ES6 Class 继承

使用 `class` 关键字和 `extends` 实现继承。

```js
class Animal {
  constructor(name) {
    this.name = name;
    this.colors = ["白色", "黑色"];
  }

  getName() {
    return this.name;
  }

  static create(name) {
    return new this(name);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  bark() {
    console.log("汪汪汪");
  }
}

const dog = new Dog("旺财", "柯基");

console.log(dog.name); // '旺财'
console.log(dog.breed); // '柯基'
console.log(dog.getName()); // '旺财'
dog.bark(); // '汪汪汪'
```

**super 关键字**：

- `super()` - 调用父类构造函数，必须在子类构造函数第一行
- `super.method()` - 调用父类方法

```js
class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  getName() {
    return `Dog: ${super.getName()}`;
  }
}

const dog = new Dog("旺财", "柯基");
console.log(dog.getName()); // 'Dog: 旺财'
```

#### 继承模式对比

| 模式           | 优点               | 缺点                       |
| -------------- | ------------------ | -------------------------- |
| 原型链继承     | 简单易用           | 引用类型属性共享，无法传参 |
| 构造函数继承   | 可传参，属性独立   | 方法无法复用               |
| 组合继承       | 常用方式，属性独立 | 调用两次父类构造函数       |
| 原型式继承     | 简单               | 引用类型共享               |
| 寄生式继承     | 可增强对象         | 方法无法复用               |
| 寄生组合式继承 | 完美方案           | 实现稍复杂                 |
| ES6 Class 继承 | 语法简洁，语义清晰 | 需要支持 ES6               |

## JavaScript 运行时与浏览器

### 进程与线程

### 浏览器内核

### js为什么是单线程的

### 浏览器的事件循环
