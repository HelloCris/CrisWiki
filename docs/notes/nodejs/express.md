# Express

## Express简介

### 什么是Express

官方给出的概念：Express是基于 Node.js 平台，快速、开放、极简的 Web 开发框架。  
通俗理解：Express的作用和 Node.js 内置的 http 模块类似，是专门用来创建 Web 服务器的。  
Express的本质：就是一个 npm 上的第三方包，提供了快速创建 Web 服务器的便捷方法。  
Express的中文官网：[http://www.Expressjs.com.cn/](http://www.Expressjs.com.cn/)

### Express能做什么

对于前端程序员来说，最常见的两种服务器，分别是：

- Web 网站服务器：专门对外提供 Web 网页资源的服务器。
- API 接口服务器：专门对外提供 API 接口的服务器。

使用 Express，我们可以方便、快速的创建 Web 网站的服务器或 API 接口的服务器。

## Express的基本使用

### 安装

在项目所处的目录中，运行如下的终端命令，即可将 Express安装到项目中使用：

```bash
npm i Express@4.17.1
```

### 创建Web服务器

```js
// 1. 导入 Express
const Express = require("Express");

// 2. 创建 web 服务器
const app = Express();

// 3. 调用 app.listen(端口号, 启动成功后的回调函数)，启动服务器
app.listen(80, () => {
  console.log("Expressserver running at http://127.0.0.1");
});
```

### 监听GET请求

通过 `app.get()` 方法，可以监听客户端的 GET 请求，具体的语法格式如下：

```js
// 参数1: 客户端请求的 URL 地址
// 参数2: 请求对应的处理函数
// req: 请求对象 (包含了与请求相关的属性与方法)
// res: 响应对象 (包含了与响应相关的属性与方法)
app.get("请求URL", function (req, res) {
  /*处理函数*/
});
```

### 监听POST请求

通过 `app.post()` 方法，可以监听客户端的 POST 请求，具体的语法格式如下：

```js
// 参数1: 客户端请求的 URL 地址
// 参数2: 请求对应的处理函数
// req: 请求对象 (包含了与请求相关的属性与方法)
// res: 响应对象 (包含了与响应相关的属性与方法)
app.post("请求URL", function (req, res) {
  /*处理函数*/
});
```

### 把内容响应给客户端

通过 `res.send()` 方法，可以把处理好的内容，发送给客户端：

```js
app.get("/user", (req, res) => {
  // 向客户端发送 JSON 对象
  res.send({ name: "zs", age: 20, gender: "男" });
});

app.post("/user", (req, res) => {
  // 向客户端发送文本内容
  res.send("请求成功");
});
```

### 获取URL的查询参数

通过 `req.query` 对象，可以访问到客户端通过 **查询字符串** 的形式，发送到服务器的参数：

```js
app.get("/", (req, res) => {
  // req.query 默认是一个空对象
  // 客户端使用 ?name=zs&age=20 这种查询字符串形式，发送到服务器的参数，
  // 可以通过 req.query 对象访问到，例如：
  // req.query.name   req.query.age
  console.log(req.query);
});
```

### 获取URL的动态参数

通过 `req.params` 对象，可以访问到 URL 中，通过 `:` 匹配到的 **动态参数**：

```js
// URL 地址中，可以通过 :参数名 的形式，匹配动态参数值
app.get("/user/:id", (req, res) => {
  // req.params 默认是一个空对象
  // 里面存放着通过 : 动态匹配到的参数值
  console.log(req.params);
});
```

> req.params中的动态参数可以是多个，例如：/user/:id/:name

## Express托管静态资源

### express.static()

express 提供了一个非常好用的函数，叫做 `express.static()`，通过它，我们可以非常方便地创建一个静态资源服务器。  
例如，通过如下代码就可以将 public 目录下的图片、CSS 文件、JavaScript 文件对外开放访问了：

```js
app.use(express.static("public"));
```

现在，你就可以访问 public 目录中的所有文件了：

- http://localhost:3000/images/bg.jpg
- http://localhost:3000/css/style.css
- http://localhost:3000/js/login.js

::: warning ⚠️ 注意

Express 在指定的静态目录中查找文件，并对外提供资源的访问路径。因此，存放静态文件的目录名不会出现在 URL 中。

:::

### 托管多个静态资源目录

如果要托管多个静态资源目录，请多次调用 `express.static()` 函数：

```js
app.use(express.static("public"));
app.use(express.static("files"));
```

访问静态资源文件时，`express.static()` 函数会根据目录的添加顺序查找所需的文件。

### 挂载路径前缀

如果希望在托管的静态资源访问路径之前，挂载路径前缀，则可以使用如下的方式：

```js
app.use("/public", express.static("public"));
```

现在，你就可以通过带有 `/public` 前缀地址来访问 public 目录中的文件了：

- http://localhost:3000/public/images/kitten.jpg
- http://localhost:3000/public/css/style.css
- http://localhost:3000/public/js/app.js

## nodemon工具

1. **为什么要使用nodemon**

在编写调试 Node.js 项目的时候,如果修改了项目的代码,则需要频繁的手动 close 掉,然后再重新启动,非常繁琐。
现在,我们可以使用 nodemon (https://www.npmjs.com/package/nodemon) 这个工具,它能够监听项目文件的变动,当代码被修改后,nodemon 会自动帮我们重启项目,极大方便了开发和调试。

2. **安装nodemon**

在终端中,运行如下命令,即可将 nodemon 安装为全局可用的工具:

```bash
npm install -g nodemon
```

3. **使用nodemon**

当基于 Node.js 编写了一个网站应用的时候，传统的方式是运行 `node app.js` 命令来启动项目。这样做的坏处是：代码被修改之后，需要手动重启项目。
现在,我们可以将 `node` 命令替换为 `nodemon` 命令,使用 `nodemon app.js` 来启动项目。这样做的好处是：代码被修改之后，会被 nodemon 监听到，从而实现自动重启项目的效果。

```bash
# 1. 传统的启动方式
node app.js

# 2. 将上面的终端命令,替换为下面的终端命令,即可实现自动重启项目的效果
nodemon app.js
```

## Express路由

### 路由的概念

在 Express 中, 路由指的是**客户端的请求与服务器处理函数之间的映射关系**。

Express 中的路由分 3 部分组成, 分别是**请求的类型、请求的 URL 地址、处理函数**, 格式如下:

```js
app.METHOD(PATH, HANDLER);
```

```js
// 匹配 GET 请求, 且请求 URL 为 /
app.get("/", function (req, res) {
  res.send("Hello World!");
});

// 匹配 POST 请求, 且请求 URL 为 /
app.post("/", function (req, res) {
  res.send("Got a POST request");
});
```

---

**路由的匹配过程**

每当一个请求到达服务器之后, 需要**先经过路由的匹配**, 只有匹配成功之后, 才会调用对应的处理函数。

在匹配时, 会按照路由的顺序进行匹配, 如果**请求类型和请求的 URL 同时匹配成功**, 则 Express 会将这次请求, 转交给对应的 `function` 函数进行处理。

![路由匹配过程](asset/routerMatch.webp)

::: warning ⚠️ 路由匹配的注意点

1. 按照定义的**先后顺序**进行匹配
2. **请求类型和请求的URL同时匹配成功**, 才会调用对应的处理函数

:::

### 路由的使用

**1. 模块化路由**

为了方便对路由进行模块化的管理, Express **不建议**将路由直接挂载到 app 上, 而是**推荐将路由抽离为单独的模块**。
将路由抽离为单独模块的步骤如下:

> 1. 创建路由模块对应的 .js 文件
> 2. 调用 `express.Router()` 函数创建路由对象
> 3. 向路由对象上挂载具体的路由
> 4. 使用 `module.exports` 向外共享路由对象
> 5. 使用 `app.use()` 函数注册路由模块

```js
var express = require("express"); // 1. 导入 express
var router = express.Router(); // 2. 创建路由对象

router.get("/user/list", function (req, res) {
  // 3. 挂载获取用户列表的路由
  res.send("Get user list.");
});
router.post("/user/add", function (req, res) {
  // 4. 挂载添加用户的路由
  res.send("Add new user.");
});

module.exports = router; // 5. 向外导出路由对象
```

**2. 注册路由模块**

```js
1 // 1. 导入路由模块
2 const userRouter = require('./router/user.js')
3
4 // 2. 使用 app.use() 注册路由模块
5 app.use(userRouter)
```

::: warning ⚠️ 注意

app.use()函数的作用就是注册全局中间件。

:::

**3. 为路由模块添加前缀**

类似于托管静态资源时, 为静态资源统一挂载访问前缀一样, 路由模块添加前缀的方式也非常简单:

```js
1 // 1. 导入路由模块
2 const userRouter = require('./router/user.js')
3
4 // 2. 使用 app.use() 注册路由模块, 并添加统一的访问前缀 /api
5 app.use('/api', userRouter)
```

## Express中间件

### 中间件的概念与格式

![中间件的概念与格式](asset/middleware01.webp)
![中间件的概念与格式](asset/middleware02.webp)
![中间件的概念与格式](asset/middleware03.webp)

### 中间件的使用

**1. 定义中间件函数**

可以通过如下的方式, 定义一个最简单的中间件函数:

```js
// 常量 mw 所指向的, 就是一个中间件函数
const mw = function (req, res, next) {
  console.log("这是一个最简单的中间件函数");
  // 注意: 在当前中间件的业务处理完毕后, 必须调用 next() 函数
  // 表示把流转关系转交给下一个中间件或路由
  next();
};
```

---

**2. 全局生效的中间件**

客户端发起的任何请求, 到达服务器之后, 都会触发的中间件, 叫做全局生效的中间件。
通过调用 `app.use(中间件函数)`, 即可定义一个全局生效的中间件, 示例代码如下:

```js
// 常量 mw 所指向的, 就是一个中间件函数
const mw = function (req, res, next) {
  console.log("这是一个最简单的中间件函数");
  // 注意: 在当前中间件的业务处理完毕后, 必须调用 next() 函数
  // 表示把流转关系转交给下一个中间件或路由
  next();
};

// 全局生效的中间件
app.use(mw);
```

---

**3. 定义全局中间件的简化形式**

```js
// 全局生效的中间件
app.use(function (req, res, next) {
  console.log("这是一个最简单的中间件函数");
  // 注意: 在当前中间件的业务处理完毕后, 必须调用 next() 函数
  // 表示把流转关系转交给下一个中间件或路由
  next();
});
```

---

**4. 中间件的作用**

多个中间件之间, 共享同一份 `req` 和 `res`。基于这样的特性, 我们可以在上游的中间件中, 统一为 `req` 或 `res` 对象添加自定义的属性或方法, 供下游的中间件或路由进行使用。

![中间件的作用](asset/middleware04.webp)

---

**5. 定义多个全局中间件**

可以使用 `app.use()` 连续定义多个全局中间件。客户端请求到达服务器之后, 会按照中间件定义的先后顺序依次进行调用, 示例代码如下:

```js
// 第1个全局中间件
app.use(function (req, res, next) {
  console.log("调用了第1个全局中间件");
  next();
});
// 第2个全局中间件
app.use(function (req, res, next) {
  console.log("调用了第2个全局中间件");
  next();
});
// 请求这个路由, 会依次触发上述两个全局中间件
app.get("/user", (req, res) => {
  res.send("Home page.");
});
```

---

**6. 局部生效的中间件**

不使用 `app.use()` 定义的中间件, 叫做局部生效的中间件, 示例代码如下:

```js
// 定义中间件函数 mw1
const mw1 = function (req, res, next) {
  console.log("这是中间件函数");
  next();
};
// mw1 这个中间件只在"当前路由中生效", 这种用法属于"局部生效的中间件"
app.get("/", mw1, function (req, res) {
  res.send("Home page.");
});
// mw1 这个中间件不会影响下面这个路由 ↓↓↓
app.get("/user", function (req, res) {
  res.send("User page.");
});
```

---

**7. 定义多个局部中间件**

可以在路由中, 通过如下两种等价的方式, 使用多个局部中间件:

```js
// 以下两种写法是"完全等价"的, 可根据自己的喜好, 选择任意一种方式进行使用
app.get("/", mw1, mw2, (req, res) => {
  res.send("Home page.");
});
app.get("/", [mw1, mw2], (req, res) => {
  res.send("Home page.");
});
```

::: warning ⚠️ 使用注意事项

1. 一定要在路由之前注册中间件
2. 客户端发送过来的请求, 可以连续调用多个中间件进行处理
3. 执行完中间件的业务代码之后, 不要忘记调用 `next()` 函数
4. 为了防止代码逻辑混乱, 调用 `next()` 函数后不要再写额外的代码
5. 连续调用多个中间件时, 多个中间件之间, 共享 `req` 和 `res` 对象

:::

### 中间件的分类

### 自定义中间件

## 使用Express写接口

### 基本流程

### 跨域问题

### JSONP
