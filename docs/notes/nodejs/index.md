# Node.js

## 初识Node.js

### 是什么

**Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。**

Node.js官网地址：[https://nodejs.org/en](https://nodejs.org/en)

![Node.js运行环境](asset/nodejsInfo.webp)

::: warning ⚠️ 注意

1. 浏览器是 JavaScript 的前端运行环境。
2. Node.js 是 JavaScript 的后端运行环境。
3. Node.js 中无法调用 DOM 和 BOM 等浏览器内置 API。

:::

### 可以做什么

1. 基于 [Express框架](http://www.Expressjs.com.cn/)，可以快速构建 Web 应用
2. 基于 [Electron 框架](https://electronjs.org/)，可以构建跨平台的桌面应用
3. 基于 [restify 框架](http://restify.com/)，可以快速构建 API 接口项目
4. 读写和操作数据库、创建实用的命令行工具辅助前端开发、etc...

### 学习路径

> JavaScript 基础语法 + Node.js 内置 API 模块（fs、path、http等）+ 第三方 API 模块（Express、mysql 等）

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

**path 模块**是 Node.js 官方提供的、用来处理路径的模块。它提供了一系列的方法和属性，用来满足用户对路径的处理需求。

例如：

- **path.join()** 方法，用来将多个路径片段拼接成一个完整的路径字符串
- **path.basename()** 方法，用来从路径字符串中，将文件名解析出来

如果要在 JavaScript 代码中，使用 path 模块来处理路径，则需要使用如下的方式先导入它：

```js
const path = require("path");
```

### 路径拼接

1. **path.join() 的语法格式**

使用 `path.join()` 方法，可以把多个路径片段拼接为完整的路径字符串，语法格式如下：

```js
path.join([...paths]);
```

**参数解读：**

- `...paths <string>` 路径片段的序列
- 返回值: `<string>`

2. **path.join() 的代码示例**

使用 `path.join()` 方法，可以把多个路径片段拼接为完整的路径字符串：

```js
const pathStr = path.join("/a", "/b/c", "..", "./d", "e");
console.log(pathStr); // 输出 \a\b\d\e

const pathStr2 = path.join(__dirname, "./files/1.txt");
console.log(pathStr2); // 输出 当前文件所处目录\files\1.txt
```

::: warning ⚠️ 注意

> 今后凡是涉及到路径拼接的操作，都要使用 `path.join()` 方法进行处理。不要直接使用 `+` 进行字符串的拼接。

:::

### 获取路径中的文件名

1. **path.basename() 的语法格式**

使用 `path.basename()` 方法，可以获取路径中的最后一部分，经常通过这个方法获取路径中的文件名，语法格式如下：

```js
path.basename(path[, ext])
```

**参数解读：**

- `path <string>` 必选参数，表示一个路径的字符串
- `ext <string>` 可选参数，表示文件扩展名
- 返回: `<string>` 表示路径中的最后一部分

2. **path.basename() 的代码示例**

使用 `path.basename()` 方法，可以从一个文件路径中，获取到文件的名称部分：

```js
const fpath = "/a/b/c/index.html"; // 文件的存放路径

var fullName = path.basename(fpath);
console.log(fullName); // 输出 index.html

var nameWithoutExt = path.basename(fpath, ".html");
console.log(nameWithoutExt); // 输出 index
```

### 获取路径中的文件扩展名

1. **path.extname() 的语法格式**

使用 `path.extname()` 方法，可以获取路径中的扩展名部分，语法格式如下：

```js
path.extname(path);
```

**参数解读：**

- `path <string>` 必选参数，表示一个路径的字符串
- 返回: `<string>` 返回得到的扩展名字符串

## http模块

### 什么是http模块

**回顾：什么是客户端、什么是服务器？**
在网络节点中，负责消费资源的电脑，叫做**客户端**；负责对外提供网络资源的电脑，叫做**服务器**。

**http 模块**是 Node.js 官方提供的、用来创建 web 服务器的模块。通过 http 模块提供的 `http.createServer()` 方法，就能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务。

如果要希望使用 http 模块创建 Web 服务器，则需要先导入它：

```js
const http = require("http");
```

服务器和普通电脑的**区别**在于：服务器上安装了 **web 服务器软件**，例如：IIS、Apache 等。通过安装这些服务器软件，就能把一台普通的电脑变成一台 web 服务器。

在 Node.js 中，我们**不需要使用** IIS、Apache 等这些**第三方 web 服务器软件**。因为我们可以基于 Node.js 提供的 http 模块，**通过几行简单的代码，就能轻松的手写一个服务器软件**，从而对外提供 web 服务。

### 服务器相关概念

#### IP地址

**IP 地址**就是互联网上每台计算机的唯一地址，因此 IP 地址具有唯一性。如果把“个人电脑”比作“一台电话”，那么“IP 地址”就相当于“电话号码”，只有在知道对方 IP 地址的前提下，才能与对应的电脑之间进行数据通信。

**IP 地址的格式**：通常用“点分十进制”表示成 `(a.b.c.d)` 的形式，其中，a,b,c,d 都是 0~255 之间的十进制整数。例如：用点分十进表示的 IP 地址 (192.168.1.1)

**注意：**

1. 互联网中每台 Web 服务器，都有自己的 IP 地址，例如：大家可以在 Windows 的终端中运行 `ping www.baidu.com` 命令，即可查看到百度服务器的 IP 地址。
2. 在开发期间，自己的电脑既是一台服务器，也是一个客户端，为了方便测试，可以在自己的浏览器中输入 `127.0.0.1` 这个 IP 地址，就能把自己的电脑当做一台服务器进行访问了。

#### 域名和域名服务器

尽管 IP 地址能够唯一地标记网络上的计算机，但 IP 地址是一长串数字，不直观，而且不便于记忆，于是人们又发明了另一套字符型的地址方案，即所谓的**域名（Domain Name）地址**。

**IP 地址和域名是一一对应的关系**，这份对应关系存放在一种叫做**域名服务器(DNS, Domain name server)**的电脑中。使用者只需通过好记的域名访问对应的服务器即可，对应的转换工作由域名服务器实现。因此，**域名服务器就是提供 IP 地址和域名之间的转换服务的服务器。**

**注意：**

1. 单纯使用 IP 地址，互联网中的电脑也能够正常工作。但是有了域名的加持，能让互联网的世界变得更加方便。
2. 在开发测试期间，`127.0.0.1` 对应的域名是 `localhost`，它们都代表我们自己的这台电脑，在使用效果上没有任何区别。

#### 端口号

计算机中的端口号，就好像是现实生活中的门牌号一样。通过门牌号，外卖小哥可以在整栋大楼众多的房间中，准确把外卖送到你的手中。

同样的道理，在一台电脑中，可以运行成百上千个 web 服务。每个 web 服务都对应一个唯一的端口号。客户端发送过来的网络请求，通过端口号，可以被准确地交给**对应的 web 服务**进行处理。

<img src="./asset/serverPort.webp" alt="serverPort" class="restrict-pic-on-pc" />

::: warning ⚠️ 注意

> 1. 每个端口号不能同时被多个 web 服务占用。
> 2. 80端口对应http服务；443端口对应https服务。

:::

### 创建最基本的web服务器

#### 基本步骤

1. 导入 http 模块
2. 创建 web 服务器实例
3. 为服务器实例绑定 **request** 事件，监听客户端的请求
4. 启动服务器

**代码示例：**

```js
// 1. 导入 http 模块
const http = require("http");

// 2. 创建 web 服务器实例
const server = http.createServer();

// 3. 为服务器实例绑定 request 事件，监听客户端的请求
server.on("request", function (req, res) {
  console.log("Someone visit our web server.");
});

// 4. 启动服务器
server.listen(8080, function () {
  console.log("server running at http://127.0.0.1:8080");
});
```

#### req请求对象

只要服务器接收到了客户端的请求，就会调用通过 `server.on()` 为服务器绑定的 **request** 事件处理函数。

如果想在事件处理函数中，访问与客户端相关的**数据或属性**，可以使用如下的方式：

```js
server.on("request", (req) => {
  // req 是请求对象，它包含了与客户端相关的数据和属性，例如：
  // req.url 是客户端请求的 URL 地址
  // req.method 是客户端的 method 请求类型
  const str = `Your request url is ${req.url}, and request method is ${req.method}`;
  console.log(str);
});
```

#### res响应对象

在服务器的 request 事件处理函数中，如果想访问与服务器相关的**数据或属性**，可以使用如下的方式：

```js
server.on("request", (req, res) => {
  // res 是响应对象，它包含了与服务器相关的数据和属性，例如：
  // 要发送到客户端的字符串
  const str = `Your request url is ${req.url}, and request method is ${req.method}`;
  // res.end() 方法的作用：
  // 向客户端发送指定的内容，并结束这次请求的处理过程
  res.end(str);
});
```

::: info 设置响应头防止中文乱码

```js
// 为了防止中文显示乱码的问题，需要设置响应头 Content-Type 的值为 text/html; charset=utf-8
res.setHeader("Content-Type", "text/html; charset=utf-8");
```

:::

### 根据不同的URL响应不同的html内容

- 核心实现步骤：
  1. 获取请求的 **url** 地址
  2. 设置默认的响应内容为 **404 Not found**
  3. 判断用户请求的是否为 `/` 或 `/index.html` 首页
  4. 判断用户请求的是否为 `/about.html` 关于页面
  5. 设置 **Content-Type** 响应头，防止中文乱码
  6. 使用 `res.end()` 把内容响应给客户端

## 模块化

### 模块分类与加载

**Node.js 中根据模块来源的不同，将模块分为了 3 大类，分别是：**

- **内置模块**（内置模块是由 Node.js 官方提供的，例如 `fs`、`path`、`http` 等）
- **自定义模块**（用户创建的每个 `.js` 文件，都是自定义模块）
- **第三方模块**（由第三方开发出来的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，**使用前需要先下载**）

> 使用强大的 `require()` 方法，可以加载需要的**内置模块**、**用户自定义模块**、**第三方模块**进行使用。
>
> **注意：** 使用 `require()` 方法加载其它模块时，会执行被加载模块中的代码。
>
> 使用require()加载用户自定义模块时可以省略.js后缀。

### 模块作用域

和函数作用域类似，在自定义模块中定义的**变量、方法**等成员，**只能在当前模块内被访问**，这种**模块级别的访问限制**，叫做**模块作用域**。

模块作用域的好处：防止全局变量污染。

### 向外共享模块作用域中的成员

#### module对象

在每个 `.js` 自定义模块中都有一个 `module` 对象，它里面**存储了和当前模块有关的信息**，打印如下：

```javascript
console.log(module);
```

**终端输出结果（Node.js 执行后打印的 module 对象）：**

```js
Module {
  id: '.',
  path: 'C:\\Users\\cris\\Desktop\\node\\day2\\code',
  exports: {},
  parent: null,
  filename: 'C:\\Users\\cris\\Desktop\\node\\day2\\code\\03.module对象.js',
  loaded: false,
  children: [],
  paths: [
    'C:\\Users\\cris\\Desktop\\node\\day2\\code\\node_modules',
    'C:\\Users\\cris\\Desktop\\node\\day2\\node_modules',
    'C:\\Users\\cris\\Desktop\\node\\node_modules',
    'C:\\Users\\cris\\Desktop\\node_modules',
    'C:\\Users\\cris\\node_modules',
    'C:\\Users\\node_modules',
    'C:\\node_modules'
  ]
}
```

**关键信息标注：**

- `id: '.'` → 表示当前模块是主模块或入口模块。
- `filename: '...\\03.module对象.js'` → 当前模块文件的绝对路径。
- `paths: [...]` → Node.js 查找模块时的搜索路径列表（从当前目录逐级向上查找 `node_modules`）。

#### module.export对象

在自定义模块中，可以使用 `module.exports` 对象，将模块内的成员共享出去，供外界使用。  
外界用 `require()` 方法导入自定义模块时，得到的就是 `module.exports` 所指向的对象。

#### export对象

由于 `module.exports` 单词写起来比较复杂，为了简化向外共享成员的代码，Node 提供了 `exports` 对象。默认情况下，`exports` 和 `module.exports` 指向同一个对象。最终共享的结果，还是以 `module.exports` 指向的对象为准。

✅ 正确用法示例：

```js
// 方式一：推荐（清晰明确）
module.exports = {
  name: "Alice",
  sayHi() {
    console.log("Hello");
  },
};

// 方式二：也可行（等价于操作 module.exports）
exports.name = "Alice";
exports.sayHi = function () {
  console.log("Hello");
};
```

❌ 错误用法：

```js
// 这样会导致 exports 不再指向 module.exports，导出为空！
exports = {
  name: "Alice",
};
```

#### module.export和export的误区

**时刻谨记，`require()` 模块时，得到的永远是 `module.exports` 指向的对象：**

> **为了防止混乱，建议大家不要在同一个模块中同时使用 `exports` 和 `module.exports`**

**代码示例1：**

```js
exports.username = "zs";
module.exports = {
  gender: "男",
  age: 22,
};
```

-> 输出：

```js
{ gender: '男', age: 22 }
```

> 💡 解释：虽然先给 `exports` 添加了属性，但随后 `module.exports` 被重新赋值为新对象，因此最终导出的是这个新对象。`exports` 的修改被覆盖。

---

**代码示例2：**

```js
module.exports.username = "zs";
exports = {
  gender: "男",
  age: 22,
};
```

-> 输出：

```js
{
  username: "zs";
}
```

> 💡 解释：`module.exports.username = 'zs'` 成功添加属性；但 `exports = {...}` 只是让 `exports` 变量指向新对象，不影响 `module.exports`，所以导出仍是最初那个含 `username` 的对象。

---

**代码示例3：**

```js
exports.username = "zs";
module.exports.gender = "男";
```

-> 输出：

```js
{ username: 'zs', gender: '男' }
```

> 💡 解释：两者操作的是同一个对象（因为初始时 `exports === module.exports`），所以属性合并成功。

---

**代码示例4：**

```js
exports = {
  username: "zs",
  gender: "男",
};
module.exports = exports;
module.exports.age = "22";
```

-> 输出：

```js
{ username: 'zs', gender: '男', age: '22' }
```

> 💡 解释：这里手动将 `module.exports` 指向了 `exports` 所指向的新对象，然后继续在该对象上添加属性，因此最终导出包含所有三个字段。

### Node.js 的模块化规范—commonjs

**Node.js 遵循了 CommonJS 模块化规范，CommonJS 规定了模块的特性和各模块之间如何相互依赖。**

- CommonJS 规定：
  1. 每个模块内部，`module` 变量代表当前模块。
  2. `module` 变量是一个对象，它的 `exports` 属性（即 `module.exports`）是对外的接口。
  3. 加载某个模块，其实是加载该模块的 `module.exports` 属性。`require()` 方法用于加载模块。

## npm 与包

### npm 了解

国外有一家 IT 公司，叫做 **npm, Inc.**。这家公司旗下有一个非常著名的网站：**[https://www.npmjs.com/](https://www.npmjs.com/)**，它是**全球最大的包共享平台**。

**npm, Inc. 公司**提供了一个地址为 **[https://registry.npmjs.org/](https://registry.npmjs.org/)** 的服务器，来对外共享所有的包，我们可以从这个服务器上下载自己所需要的包。

- 从 **[https://www.npmjs.com/](https://www.npmjs.com/)** 网站上搜索自己所需要的包
- 从 **[https://registry.npmjs.org/](https://registry.npmjs.org/)** 服务器上下载自己需要的包

**npm, Inc. 公司**提供了一个包管理工具，我们可以使用这个包管理工具，从 **[https://registry.npmjs.org/](https://registry.npmjs.org/)** 服务器把需要的包下载到本地使用。

这个包管理工具的名字叫做 **Node Package Manager**（简称 **npm 包管理工具**），这个包管理工具随着 Node.js 的安装包一起被安装到了用户的电脑上。

大家可以在终端中执行 **`npm -v`** 命令，来查看自己电脑上所安装的 npm 包管理工具的版本号。

### npm install //npm i

初次装包完成后，在项目文件夹下多一个叫做 **node_modules** 的文件夹和 **package-lock.json** 的配置文件。

其中：

- **node_modules 文件夹**用来存放所有已安装到项目中的包。`require()` 导入第三方包时，就是从这个目录中查找并加载包。
- **package-lock.json 配置文件**用来记录 `node_modules` 目录下的每一个包的下载信息，例如包的名字、版本号、下载地址等。

::: warning ⚠️ 注意

> 程序员不要手动修改 `node_modules` 或 `package-lock.json` 文件中的任何代码，npm 包管理工具会自动维护它们。

:::

---

默认情况下，使用 `npm install` 命令安装包的时候，**会自动安装最新版本的包**。如果需要安装指定版本的包，可以在包名之后，通过 **@ 符号**指定具体的版本，例如：

```bash
npm i moment@2.22.2
```

::: warning ⚠️ 注意

> `npm i` 是 `npm install` 的简写形式，功能完全相同。

:::

---

包的版本号是以“**点分十进制**”形式进行定义的，总共有三位数字，例如 **2.24.0**

其中每一位数字所代表的含义如下：

- **第1位数字：大版本**（Major Version）→ 表示重大更新，可能包含不兼容的 API 变更
- **第2位数字：功能版本**（Minor Version）→ 表示新增功能，但保持向后兼容
- **第3位数字：Bug修复版本**（Patch Version）→ 表示仅修复错误或安全漏洞，不影响现有功能

### 包管理配置文件

npm 规定，在**项目根目录**中，**必须**提供一个叫做 **package.json** 的包管理配置文件。用来记录与项目有关的一些配置信息。例如：

- 项目的名称、版本号、描述等
- 项目中都用到了哪些包
- 哪些包只在**开发期间**会用到
- 那些包在**开发和部署时**都需要用到

在**项目根目录**中，创建一个叫做 **package.json** 的配置文件，即可用来记录项目中安装了哪些包。从而方便剔除 `node_modules` 目录之后，在团队成员之间共享项目的源代码。

::: warning ⚠️ 注意

> 在项目开发中，一定要把 `node_modules` 文件夹，添加到 `.gitignore` 忽略文件中。

:::

npm 包管理工具提供了一个**快捷命令**，可以在**执行命令时所处的目录中**，快速创建 `package.json` 这个包管理配置文件：

```bash
// 作用：在执行命令所处的目录中，快速新建 package.json 文件
npm init -y
```

::: warning ⚠️ 注意

> 1. 上述命令**只能在英文的目录下成功运行！** 所以，项目文件夹的名称一定要使用英文命名，**不要使用中文，不能出现空格**。
> 2. 运行 `npm install` 命令安装包的时候，npm 包管理工具会自动把**包的名称和版本号**，记录到 `package.json` 中。

:::

### 装包、删包

`package.json` 文件中，有一个 **dependencies** 节点，专门用来记录您使用 `npm install` 命令安装了哪些包。

可以运行 `npm install` 命令（或 `npm i`）一次性安装所有的依赖包：

```bash
npm install
```

可以运行 `npm uninstall` 命令，来卸载指定的包：

```bash
npm uninstall moment
```

> **注意：** `npm uninstall` 命令执行成功后，会把卸载的包，自动从 `package.json` 的 `dependencies` 中移除掉。

### devDependencies

如果某些包**只在项目开发阶段**会用到，在**项目上线之后不会用到**，则建议把这些包记录到 **devDependencies** 节点中。

与之对应的，如果某些包在**开发**和**项目上线之后**都需要用到，则建议把这些包记录到 **dependencies** 节点中。

您可以使用如下的命令，将包记录到 **devDependencies** 节点中：

```bash
// 安装指定的包，并记录到 devDependencies 节点中
npm i 包名 -D

// 注意：上述命令是简写形式，等价于下面完整的写法：
npm install 包名 --save-dev
```

> 判断应该装包到devDependencies节点还是dependencies节点，直接npmjs.com官网查一下对应包的安装命令即可。

### npm镜像

> 淘宝在国内搭建了一个服务器，专门把国外官方服务器上的包同步到国内的服务器，然后在国内提供下包的服务。从而极大的提高了下包的速度。
>
> **镜像（Mirroring）** 是一种文件存储形式，一个磁盘上的数据在另一个磁盘上存在一个完全相同的副本即为镜像。

- **国内用户** → 从国外服务器下包 → **国外 npm 官方服务器**
- **国内用户** → 从国内服务器下包 → **国内淘宝npm镜像服务器** →(定期同步)→ **国外 npm 官方服务器**

下包的镜像源，指的就是**下包的服务器地址**。

```bash
# 查看当前的下包镜像源
npm config get registry

# 将下包的镜像源切换为淘宝镜像源
npm config set registry=https://registry.npmmirror.com/

# 检查镜像源是否下载成功
npm config get registry
```

::: warning ⚠️ 注意
目前（2024年起）淘宝已启用新域名：**https://registry.npmmirror.com/**  
原 `registry.npm.taobao.org` 已逐步停用，请优先使用新地址。
:::

#### nrm插件

为了更方便的切换下包的镜像源，我们可以安装 **nrm** 这个小工具，利用 nrm 提供的终端命令，可以快速查看和切换下包的镜像源。

```bash
# 通过 npm 包管理器，将 nrm 安装为全局可用的工具
npm i nrm -g

# 查看所有可用的镜像源
nrm ls

# 将下包的镜像源切换为 taobao 镜像
nrm use taobao
```

### 包的分类

#### 项目包

那些被安装到**项目的 `node_modules` 目录**中的包，都是**项目包**。

项目包又分为两类，分别是：

- **开发依赖包**（被记录到 `devDependencies` 节点中的包，只在开发期间会用到）
- **核心依赖包**（被记录到 `dependencies` 节点中的包，在开发期间和项目上线之后都会用到）

```bash
npm i 包名 -D    # 开发依赖包（会被记录到 devDependencies 节点下）
npm i 包名       # 核心依赖包（会被记录到 dependencies 节点下）
```

#### 全局包

在执行 `npm install` 命令时，如果提供了 `-g` 参数，则会把包安装为**全局包**。

全局包会被安装到：  
`C:\Users\用户目录\AppData\Roaming\npm\node_modules` 目录下。（Windows 系统路径示例）

```bash
npm i 包名 -g        # 全局安装指定的包
npm uninstall 包名 -g # 卸载全局安装的包
```

::: warning ⚠️ 注意

1. 只有**工具性质的包**，才有全局安装的必要性。因为它们提供了好用的终端命令。  
     例如：`nodemon`, `eslint`, `create-react-app`, `vue-cli`, `typescript (tsc)` 等。
2. 判断某个包是否需要全局安装后才能使用，可以**参考官方提供的使用说明**即可。

:::

### 规范的包结构

一个规范的包，它的组成结构，必须符合以下 3 点要求：

1. 包必须以**单独的目录**而存在
2. 包的顶级目录下要必须包含 **package.json** 这个包管理配置文件
3. **package.json** 中必须包含 **name**, **version**, **main** 这三个属性，分别代表**包的名字、版本号、包的入口**。

### 发布包-开发属于自己的包

#### 本地创建包

1. 新建 cris-tools 文件夹, 作为包的根目录
2. 在 cris-tools 文件夹中, 新建如下三个文件:
   - package.json (包管理配置文件)
   - index.js (包的入口文件)
   - README.md (包的说明文档)

3. 初始化 package.json 文件

```json
{
  "name": "cris-tools",
  "version": "1.0.0",
  "main": "index.js",
  "description": "提供了格式化时间, HTMLEscape的功能",
  "keywords": ["cris", "dateFormat", "escape"],
  "license": "ISC"
}
```

#### 远程包发布/删除

::: info 1. 注册 npm 账号

1. 访问 https://www.npmjs.com/ 网站, 点击 sign up 按钮, 进入注册用户界面
2. 填写账号相关的信息: Full Name、Public Email、Username、Password
3. 点击 Create an Account 按钮, 注册账号
4. 登录邮箱, 点击验证链接, 进行账号的验证

:::

---

::: info 2. 登录 npm 账号

npm 账号注册完成后, 可以在终端中执行 `npm login` 命令, 依次输入用户名、密码、邮箱后, 即可登录成功。

```bash
C:\Users>npm login
Username: lih314
Password:
Email: (this IS public) front@itcast.cn
Logged in as lih314 on https://registry.npmjs.org/.
```

::: warning ⚠️ 注意

> 在运行 `npm login` 命令之前, 必须先把下包的服务器地址切换为 npm 的官方服务器。否则会导致发布包失败!

:::

---

::: info 3. 把包发布到 npm 上

将终端切换到包的根目录之后, 运行 `npm publish` 命令, 即可将包发布到 npm 上 (注意: 包名不能雷同)。

```bash
C:\Users\cris\Desktop\cris-tools>npm publish
npm notice
npm notice package: cris-tools@1.0.0
npm notice === Tarball Contents ===
npm notice 677B src/dateFormat.js
npm notice 741B src/htmlEscape.js
npm notice 349B index.js
npm notice 229B package.json
npm notice 816B README.md
npm notice === Tarball Details ===
npm notice name:          cris-tools
npm notice version:       1.0.1
npm notice package size:  1.4 kB
npm notice unpacked size: 2.8 kB
npm notice shasum:        4683fd9e9f14e8a8656a7ebfa46c59e576525dcf
npm notice integrity:     sha512-bOmS3ZPe2vxvAu[...]g2MNxaJLYePFA==
npm notice total files:   5
npm notice
+ cris-tools@1.0.1

```

:::

---

::: info 4. 删除已发布的包

运行 `npm unpublish 包名 --force` 命令, 即可从 npm 删除已发布的包。

```bash
C:\Users\cris\Desktop\cris-tools>npm unpublish cris-tools --force
npm WARN using --force I sure hope you know what you are doing.
- cris-tools
```

::: warning ⚠️ 注意

1. `npm unpublish` 命令只能删除 **72 小时以内**发布的包
2. `npm unpublish` 删除的包, 在 **24 小时内**不允许重复发布
3. 发布包的时候要慎重, **尽量不要往 npm 上发布没有意义的包!**

:::

## 模块的加载机制

### 优先从缓存中加载

模块在第一次加载后会被缓存。这也意味着多次调用 require() 不会导致模块的代码被执行多次。

::: warning ⚠️ 注意

不论是内置模块、用户自定义模块、还是第三方模块，它们都会优先从缓存中加载，从而提高模块的加载效率。

:::

### 内置模块的加载机制

内置模块是由 Node.js 官方提供的模块，内置模块的加载优先级最高。

例如，require('fs') 始终返回内置的 fs 模块，即使在 node_modules 目录下有名字相同的包也叫做 fs。

### 自定义模块的加载机制

使用 require() 加载自定义模块时，必须指定以 / 或 ./ 开头的路径标识符。在加载自定义模块时，如果没有指定 / 或 ./ 这样的路径标识符，则 node 会把它当作内置模块或第三方模块进行加载。

同时，在使用 require() 导入自定义模块时，如果省略了文件的扩展名，则 Node.js 会按顺序分别尝试加载以下的文件：

1. 按照确切的文件名进行加载
2. 补全 .js 扩展名进行加载
3. 补全 .json 扩展名进行加载
4. 补全 .node 扩展名进行加载
5. 加载失败，终端报错

### 第三方模块的加载机制

如果传递给 require() 的模块标识符不是一个内置模块，也没有以 ‘./’ 或 ‘../’ 开头，则 Node.js 会从当前模块的父目录开始，尝试从 /node_modules 文件夹中加载第三方模块。

如果没有找到对应的第三方模块，则移动到再上一层父目录中，进行加载，直到文件系统的根目录。

例如，假设在 'C:\Users\cris\project\foo.js' 文件里调用了 require('tools')，则 Node.js 会按以下顺序查找：

1. C:\Users\cris\project\node_modules\tools
2. C:\Users\cris\node_modules\tools
3. C:\Users\node_modules\tools
4. C:\node_modules\tools

### 目录作为模块的加载机制

当把目录作为模块标识符，传递给 require() 进行加载的时候，有三种加载方式：

1. 在被加载的目录下查找一个叫做 package.json 的文件，并寻找 main 属性，作为 require() 加载的入口
2. 如果目录里没有 package.json 文件，或者 main 入口不存在或无法解析，则 Node.js 将会试图加载目录下的 index.js 文件。
3. 如果以上两步都失败了，则 Node.js 会在终端打印错误消息，报告模块的缺失：Error: Cannot find module 'xxx'

## Express

### Express简介

#### 什么是Express

官方给出的概念：Express是基于 Node.js 平台，快速、开放、极简的 Web 开发框架。  
通俗理解：Express的作用和 Node.js 内置的 http 模块类似，是专门用来创建 Web 服务器的。  
Express的本质：就是一个 npm 上的第三方包，提供了快速创建 Web 服务器的便捷方法。  
Express的中文官网：[http://www.Expressjs.com.cn/](http://www.Expressjs.com.cn/)

#### Express能做什么

对于前端程序员来说，最常见的两种服务器，分别是：

- Web 网站服务器：专门对外提供 Web 网页资源的服务器。
- API 接口服务器：专门对外提供 API 接口的服务器。

使用 Express，我们可以方便、快速的创建 Web 网站的服务器或 API 接口的服务器。

### Express的基本使用

#### 安装

在项目所处的目录中，运行如下的终端命令，即可将 Express安装到项目中使用：

```bash
npm i Express@4.17.1
```

#### 创建Web服务器

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

#### 监听GET请求

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

#### 监听POST请求

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

#### 把内容响应给客户端

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

#### 获取URL的查询参数

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

#### 获取URL的动态参数

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

### Express托管静态资源

#### express.static()

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

#### 托管多个静态资源目录

如果要托管多个静态资源目录，请多次调用 `express.static()` 函数：

```js
app.use(express.static("public"));
app.use(express.static("files"));
```

访问静态资源文件时，`express.static()` 函数会根据目录的添加顺序查找所需的文件。

#### 挂载路径前缀

如果希望在托管的静态资源访问路径之前，挂载路径前缀，则可以使用如下的方式：

```js
app.use("/public", express.static("public"));
```

现在，你就可以通过带有 `/public` 前缀地址来访问 public 目录中的文件了：

- http://localhost:3000/public/images/kitten.jpg
- http://localhost:3000/public/css/style.css
- http://localhost:3000/public/js/app.js

### nodemon工具

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

### Express路由

#### 路由的概念

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

#### 路由的使用

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

### Express中间件

#### 中间件的概念与格式

![中间件的概念与格式](asset/middleware01.webp)
![中间件的概念与格式](asset/middleware02.webp)
![中间件的概念与格式](asset/middleware03.webp)

#### 中间件的使用

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

#### 中间件的分类

#### 自定义中间件

### 使用Express写接口

#### 基本流程

#### 跨域问题

#### JSONP

## 数据库

### 数据库基本概念

### 安装并配置 MySQL

### MySQL Workbench使用

### 使用SQL管理数据库

#### 什么是 SQL

#### SQL 的增删改查语句

#### SQL的where、and和or

#### SQL排序order by

#### count函数和as关键字

### nodejs中怎么使用MySQL模块

#### 引入MySQL模块

#### 增删改查

## 前后端的身份认证

### web开发模式

### 身份认证

### session

#### 概念

#### Express-session中间件

### jwt

#### JWT

#### JWT在Express中的使用
