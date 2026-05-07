# 微信小程序

## 简介

::: info 小程序特点

- 类似于Web开发模式，入门的门槛低：基本上是类似于html+css+js；
- 可直接云端更新：微信审核，无需经过App Store等平台；
- 提升用户体验：通过提供基础能力、原生组件结合等方式，提升用户体验；
- 平台管控能力：小程序提供云端更新，通过代码上传、审核等方式，增强对开发者的管控能力；
- 双线程模型：逻辑层和渲染层分开加载，提供了管控型和安全性（沙盒环境运行JS代码，不允许执行任何和浏览器相关的接口，比如跳转页面、操作DOM等）；

:::

[微信小程序开发文档](https://developers.weixin.qq.com/miniprogram/dev/framework/release/)

小程序的开发主要分成三部分：

- 页面布局：WXML，类似HTML
- 页面样式：WXSS，几乎就是CSS（某些不支持，某些进行了增强）
- 页面脚本：JavaScript+WXS，（JS，以及WeixinScript后续学习）

### 申请APPID

1. **访问微信公众平台**  
   打开 [微信公众平台](https://mp.weixin.qq.com/)，使用微信扫描二维码登录

2. **选择小程序注册类型**  
   点击"立即注册"，选择"小程序"类型

3. **填写注册信息**  
   邮箱：一个未绑定微信公众平台的邮箱

4. **邮箱激活**  
   登录注册邮箱，点击微信发送的激活链接

5. **信息登记**  
   选择主体类型：个人/企业/政府、媒体、其他组织等

6. **完善小程序信息**  
   小程序名称（需审核）/头像/简介

7. **获取 AppID**  
   注册完成后，在"开发"-"开发管理"-"开发设置"中查看 AppID

::: info AppID的作用

- **开发调试**：在微信开发者工具中配置 AppID 进行开发
- **接口调用**：调用微信 API 需要 AppID 验证身份
- **发布上线**：小程序上线审核需要已认证的 AppID

:::

::: warning ⚠️ 注意

- 个人主体小程序功能有限，很多接口无法使用
- 企业主体小程序需要微信认证（300元/年）
- 小程序名称需符合微信命名规范，不能与已存在的小程序名称重复
- AppID 泄露不会造成安全风险，但建议妥善保管

:::

[微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)

## 小程序架构和配置

### 代码组织结构

小程序代码组织结构分为三个级别：应用级别、页面级别和组件级别。

| 级别         | 文件           | 说明                                             |
| ------------ | -------------- | ------------------------------------------------ |
| **应用级别** | app.js         | 创建 App 实例的代码，包含全局逻辑和生命周期      |
|              | app.json       | 全局配置，如 window、tabbar、页面路由等          |
|              | app.wxss       | 全局样式，对所有页面生效                         |
| **页面级别** | page.js        | 创建 Page 实例的代码，包含页面逻辑和生命周期     |
|              | page.json      | 页面单独配置，如 window 配置、usingComponents 等 |
|              | page.wxml      | 页面的布局代码，类似于 HTML                      |
|              | page.wxss      | 页面样式，类似于 CSS                             |
| **组件级别** | component.js   | 创建 Component 实例的代码，包含组件逻辑          |
|              | component.json | 组件配置，声明依赖的其他组件                     |
|              | component.wxml | 组件的布局代码                                   |
|              | component.wxss | 组件的样式代码                                   |

### 配置文件

::: info 常见的配置文件有哪些呢？

- **project.config.json**：项目配置文件，比如项目名称、appid等；[项目配置文件](https://developers.weixin.qq.com/miniprogram/dev/devtools/projectconfig.html)
- **sitemap.json**：小程序搜索相关的；
- **app.json**：全局配置；
- **page.json**：页面配置；

:::

> [全局配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html)

| 属性       | 类型     | 必填   | 描述               |
| ---------- | -------- | ------ | ------------------ |
| **pages**  | string[] | **是** | 页面路径列表       |
| **window** | Object   | 否     | 全局的默认窗口表现 |
| **tabBar** | Object   | 否     | 底部 tab 栏的表现  |

**1. pages：页面路径列表**

- **作用**：用于指定小程序由哪些页面组成，每一项都对应一个页面的路径（含文件名）信息。
- **注意**：小程序中**所有的页面**都是必须在 `pages` 中进行注册的。

**2. window：全局的默认窗口展示**

- **作用**：用户指定窗口如何展示，其中还包含了很多其他的属性（如导航栏颜色、背景色等）。

**3. tabBar：底部tab栏的展示**

> **页面配置**

- 每一个小程序页面也可以使用 .json 文件来对本页面的窗口表现进行配置。页面中配置项在当前页面会覆盖 app.json 的 window 中相同的配置项。

### 小程序启动流程

![小程序启动流程](asset/startupProcess.webp)

::: info 界面渲染整体流程

1. 在渲染层，宿主环境会把**WXML**转化成对应的**JS对象**；
2. 将**JS对象**再次转成**真实DOM树**，交由渲染层线程渲染；
3. 数据变化时，逻辑层提供最新的变化数据，JS对象发生变化比较进行diff算法对比；
4. 将最新变化的内容反映到真实的DOM树中，更新UI；

:::

#### 注册App

- 每个小程序都需要在 app.js 中调用 App 方法。[App 方法](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html)
- 在注册时，可以绑定对应的生命周期函数，在生命周期函数中，执行对应的代码。

| 属性           | 类型     | 说明                                                                 |
| -------------- | -------- | -------------------------------------------------------------------- |
| onLaunch       | function | 生命周期回调——监听小程序初始化。                                     |
| onShow         | function | 生命周期回调——监听小程序启动或切前台。                               |
| onHide         | function | 生命周期回调——监听小程序切后台。                                     |
| onError        | function | 错误监听函数。                                                       |
| onPageNotFound | function | 页面不存在监听函数。                                                 |
| 其他           | any      | 开发者可以添加任意的函数或数据变量到 Object 参数中，用 this 可以访问 |

::: info 注册App时常做的操作

1. 判断小程序的**进入场景**
2. 监听**生命周期函数**，在生命周期中执行对应的业务逻辑，比如在某个生命周期函数中获取微信用户的信息
3. 因为App()实例只有一个，并且是**全局共享的**（单例对象），所以我们可以将一些共享数据放在这里。

:::

```js
App({
  // 当小程序初始化完成时，会触发 onLaunch（全局只触发一次）
  onLaunch: function () {
    console.log("小程序初始化完成: onLaunch");
  },
  // 当小程序启动，或从后台进入前台显示，会触发 onShow
  onShow: function (options) {
    console.log("小程序第一次启动，或者从后台进入前台, onShow");
  },
  // 当小程序从前台进入后台，会触发 onHide
  onHide: function () {
    console.log("小程序从前台进入后台, onHide");
  },
  // 当小程序发生脚本错误，或者 api 调用失败时，会触发 onError 并带上错误信息
  onError: function (msg) {
    console.log("小程序发生错误, onError", msg);
  },
  globalData: {
    title: "全局的title",
    name: "cris",
    age: 18,
    height: 1.8,
  },
});
```

```js
// 1.获取全局的app对象
const app = getApp();

Page({
  data: {
    message: "微信",
    name: "kobe",
  },
  onClick() {
    // 2.通过app.globalData.属性的方式获取
    this.setData({
      message: app.globalData.title,
      name: app.globalData.name,
    });
  },
});
```

#### 注册Page

- 小程序中的每个页面，都有一个对应的js文件，其中调用Page方法。[Page 方法](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html)
- 在注册时，可以绑定初始化数据、生命周期回调、事件处理函数等。

| 属性              | 类型     | 说明                                                                           |
| ----------------- | -------- | ------------------------------------------------------------------------------ |
| data              | Object   | 页面的初始数据                                                                 |
| onLoad            | function | 生命周期回调——监听页面加载                                                     |
| onShow            | function | 生命周期回调——监听页面显示                                                     |
| onReady           | function | 生命周期回调——监听页面初次渲染完成                                             |
| onHide            | function | 生命周期回调——监听页面隐藏                                                     |
| onUnload          | function | 生命周期回调——监听页面卸载                                                     |
| onPullDownRefresh | function | 监听用户下拉动作                                                               |
| onReachBottom     | function | 页面上拉触底事件的处理函数                                                     |
| onShareAppMessage | function | 用户点击右上角转发                                                             |
| onPageScroll      | function | 页面滚动触发事件的处理函数                                                     |
| onResize          | function | 页面尺寸改变时触发，详见 响应显示区域变化                                      |
| onTabItemTap      | function | 当前是 tab 页时，点击 tab 时触发                                               |
| 其他              | any      | 开发者可以添加任意的函数或数据到 Object 参数中，在页面的函数中用 this 可以访问 |

::: info 注册Page时常做的操作

1. 在**生命周期函数**中发送网络请求，从服务器获取数据；
2. **初始化一些数据**，以方便被wxml引用展示；
3. **监听wxml中的事件**，绑定对应的事件函数；
4. 其他一些**监听**（比如页面滚动、上拉刷新、下拉加载更多等）；

:::

```js
Page({
  // ----------- 数据 -----------
  data: {
    message: "你好啊,李银河",
  },
  // ----------- 生命周期函数 -----------
  onLoad() {
    console.log("页面加载：onLoad");
  },
  onShow() {
    console.log("页面展示：onShow");
  },
  onReady() {
    console.log("页面渲染：onReady");
  },
  onHide() {
    console.log("页面隐藏：onHide");
  },
  onUnload() {
    console.log("页面卸载：onUnload");
  },
  // ----------- 事件监听 -----------
  onClick(e) {
    console.log("按钮被点击");
  },
});
```

## 附录

### 附1: 小程序MVVM架构

::: info 为什么像MVVM框架？（开发体验角度）

- **数据驱动视图**：你只需要关注数据（`data`）的变化，界面（WXML）会自动更新。
- **双向绑定语法**：小程序使用 `{{ }}` 进行数据绑定，这与 Vue.js 等框架的语法非常相似。
- **逻辑与视图分离**：
  - **View（视图层）**：WXML 和 WXSS，负责展示。
  - **ViewModel（视图模型层）**：框架底层帮你处理了数据绑定和事件监听。
  - **Model（模型层）**：Page 或 Component 中的 `data` 和逻辑代码。

:::

::: info 为什么不是标准的MVVM框架？（底层实现角度）

- **双线程架构**：
  - **传统 MVVM（如 Vue/React 在浏览器中）**：逻辑和视图运行在同一个线程（UI 线程），直接操作 DOM（或虚拟 DOM）。
  - **小程序**：采用了**逻辑层**（JS）和**视图层**（WXML/WXSS）分开的**双线程模型**。
    - **逻辑层**运行在 JS Core 中，没有浏览器对象（如 `window`、`document`），负责处理数据。
    - **视图层**运行在 WebView 中，负责渲染界面。
  - **通信机制**：当数据变化时，逻辑层通过微信客户端（Native）作为桥梁，将数据序列化传输给视图层，视图层再重新渲染。这与 MVVM 框架中 JS 直接响应式更新 DOM 的机制不同。

:::

### 附2:
