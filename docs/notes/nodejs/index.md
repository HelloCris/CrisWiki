# Node.js

## 初识 Node.js

### 是什么

**Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。**

Node.js官网地址：[https://nodejs.org/en](https://nodejs.org/en)

![Node.js运行环境](nodejsInfo.webp)

::: warning 注意

1. 浏览器是 JavaScript 的前端运行环境。
2. Node.js 是 JavaScript 的后端运行环境。
3. Node.js 中无法调用 DOM 和 BOM 等浏览器内置 API。

:::

### 可以做什么

1. 基于 [Express 框架](http://www.expressjs.com.cn/)，可以快速构建 Web 应用
2. 基于 [Electron 框架](https://electronjs.org/)，可以构建跨平台的桌面应用
3. 基于 [restify 框架](http://restify.com/)，可以快速构建 API 接口项目
4. 读写和操作数据库、创建实用的命令行工具辅助前端开发、etc...

### 学习路径

> JavaScript 基础语法 + Node.js 内置 API 模块（fs、path、http等）+ 第三方 API 模块（express、mysql 等）

### 环境的安装

**终端（英文：Terminal）是专门为开发人员设计的，用于实现人机交互的一种方式。**

查看安装的nodejs版本号：node -v  
运行js代码：node a.js

window终端常用快捷键：

1. 使用 ↑ 键，可以快速定位到上一次执行的命令
2. 使用 tab 键，能够快速补全路径
3. 使用 esc 键，能够快速清空当前已输入的命令
4. 输入 cls 命令，可以清空终端

## fs文件系统模块

### 什么是fs文件系统模块

**fs 模块是 Node.js 官方提供的、用来操作文件的模块。它提供了一系列的方法和属性，用来满足用户对文件的操作需求。**

例如：

- `fs.readFile()` 方法，用来读取指定文件中的内容
- `fs.writeFile()` 方法，用来向指定的文件中写入内容

如果要在 JavaScript 代码中，使用 fs 模块来操作文件，则需要使用如下的方式先导入它：

```js
const fs = require("fs");
```

### 读取指定文件中的内容

使用 `fs.readFile()` 方法，可以读取指定文件中的内容，语法格式如下：

```js
fs.readFile(path[, options], callback)
```

**参数解读：**

- 参数1：**必选参数**，字符串，表示文件的路径。
- 参数2：可选参数，表示以什么**编码格式**来读取文件。
- 参数3：**必选参数**，文件读取完成后，通过回调函数拿到读取的结果。

::: info 代码示例
可以判断 `err` 对象是否为 `null`，从而知晓文件读取的结果：

```js
const fs = require("fs");
fs.readFile("./files/1.txt", "utf8", function (err, result) {
  if (err) {
    return console.log("文件读取失败！" + err.message);
  }
  console.log("文件读取成功，内容是：" + result);
});
```

:::

### 向指定的文件中写入内容

使用 `fs.writeFile()` 方法，可以向指定的文件中写入内容，语法格式如下：

```js
fs.writeFile(file, data[, options], callback)
```

**参数解读：**

- 参数1：**必选参数**，需要指定一个**文件路径的字符串**，表示文件的存放路径。
- 参数2：**必选参数**，表示要写入的内容。
- 参数3：可选参数，表示以什么格式写入文件内容，默认值是 utf8。
- 参数4：**必选参数**，文件写入完成后的回调函数。

::: info 代码示例

可以判断 `err` 对象是否为 `null`，从而知晓文件写入的结果：

```js
const fs = require("fs");
fs.writeFile("F:/files/2.txt", "Hello Node.js!", function (err) {
  if (err) {
    return console.log("文件写入失败！" + err.message);
  }
  console.log("文件写入成功！");
});
```

:::

### 路径动态拼接问题

在使用 fs 模块操作文件时，如果提供的操作路径是以 `./` 或 `../` 开头的**相对路径**时，很容易出现路径动态拼接错误的问题。

**原因：** 代码在运行的时候，会以执行 node 命令时所处的目录，动态拼接出被操作文件的完整路径。

**解决方案：** 在使用 fs 模块操作文件时，**直接提供完整的路径**，不要提供 `./` 或 `../` 开头的相对路径，从而防止路径动态拼接的问题。

```js
// 不要使用 ./ 或 ../ 这样的相对路径
fs.readFile("./files/1.txt", "utf8", function (err, dataStr) {
  if (err) return console.log("读取文件失败！" + err.message);
  console.log(dataStr);
});

// __dirname 表示当前文件所处的目录
fs.readFile(__dirname + "/files/1.txt", "utf8", function (err, dataStr) {
  if (err) return console.log("读取文件失败！" + err.message);
  console.log(dataStr);
});
```

## path路径模块

### 什么是path路径模块

### 路径拼接

### 获取路径中的文件名

### 获取路径中的文件扩展名

## http模块

### 什么是http模块

### 服务器相关概念

### 创建最基本的web服务器

### 根据不同的URL响应不同的html内容

## 模块化

### 模块分类与加载

### 模块作用域

### 向外共享模块作用域中的成员

#### module 对象

#### module.export 对象

#### export 对象

#### module.export 和 export 的误区

### Node.js 的模块化规范—commonjs

## npm 与包

### npm 了解

### npm install //npm i

### 包管理配置文件

### 装包、删包

### devDependencies

### 淘宝 npm 镜像——下包速度慢问题解决

### 包的分类

### 规范的包结构

### 发布包 – 开发属于自己的包

## 模块的加载机制

## express

### express 简介

### express 的基本使用

### express 托管静态资源

### nodemon 工具

### express 路由

#### 路由的概念

#### 路由的使用

### express 中间件

#### 中间件的概念与格式

#### 中间件的使用

#### 中间件的分类

#### 自定义中间件

### 使用 express 写接口

#### 基本流程

#### 跨域问题

#### JSONP

## 数据库

### 数据库基本概念

### 安装并配置 MySQL

### MySQL Workbench 使用

### 使用 SQL 管理数据库

#### 什么是 SQL

#### SQL 的增删改查语句

#### SQL 的 where、and 和 or

#### SQL 排序 order by

#### count 函数和 as 关键字

### nodejs 中怎么使用 MySQL 模块

#### 引入 MySQL 模块

#### 增删改查

## 前后端的身份认证

### web 开发模式

### 身份认证

### session

#### 概念

#### express-session 中间件

### jwt

#### JWT

#### JWT 在 express 中的使用
