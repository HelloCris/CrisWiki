# Vue2 基础

## 认识vue

- Vue特点: 解耦视图和数据、可复用的组件、前端路由技术、状态管理、虚拟DOM。
- Vue.js安装方式：1. 直接CDN引入 2. 下载引入 3. NPM安装
- Vue中的MVVM架构：
  - M：model数据层，可能是死数据，也可能来自于服务器。
  - V：view视图层，前端通常指dom层。主要作用给用户展示各种信息
  - VM：vuemodel视图模型层，是view和model沟通的桥梁。主要作用：一、实现了data binding（数据绑定），将model的改变实时的反应到view中。二、实现dom listener（dom监听），当dom发生某些事件时，可以根据需要改变对应的data。

::: info 创建vue实例时可传入的options

| 属性         | 类型                 | 说明                             |
| ------------ | -------------------- | -------------------------------- |
| `el`         | String / HTMLElement | 挂载点，指定Vue实例管理的DOM元素 |
| `data`       | Object / Function    | 数据对象，Vue实例的数据来源      |
| `methods`    | Object               | 方法对象，定义Vue实例的方法      |
| `computed`   | Object               | 计算属性，基于依赖缓存的计算结果 |
| `components` | Object               | 组件对象，注册局部组件           |
| `filters`    | Object               | 过滤器对象，注册局部过滤器       |
| `watch`      | Object               | 监听属性，监听数据变化并执行回调 |
| `props`      | Array / Object       | 属性列表，接收父组件传递的数据   |
| `template`   | String               | 模板字符串，组件的模板内容       |
| `render`     | Function             | 渲染函数，createElement函数      |

:::

::: info 计算属性

计算属性是基于依赖缓存的计算结果，当依赖的属性发生变化时，计算属性会自动更新。  
每个计算属性都包含一个getter和一个setter。默认只有getter。

> **计算属性的缓存：**  
> methods和computed看起来都可以实现我们的功能，那么为什么还要多一个计算属性这个东西呢？原因：计算属性会进行缓存，如果多次使用时，计算属性只会调用一次。

:::

::: info 生命周期钩子

| 属性            | 类型     | 说明           |
| --------------- | -------- | -------------- |
| `beforeCreate`  | Function | 实例创建前钩子 |
| `created`       | Function | 实例创建后钩子 |
| `beforeMount`   | Function | 挂载前钩子     |
| `mounted`       | Function | 挂载后钩子     |
| `beforeUpdate`  | Function | 更新前钩子     |
| `updated`       | Function | 更新后钩子     |
| `beforeDestroy` | Function | 销毁前钩子     |
| `destroyed`     | Function | 销毁后钩子     |

![VueLifecycleHooks](asset/VueLifecycleHooks.png)
:::

## vue基础语法

Mustache语法(也就是双大括号)，双大括号中也可以进行简单的运算，比如：加减乘除等。

### 指令

| 指令      | 作用描述                                                                                                                |
| --------- | ----------------------------------------------------------------------------------------------------------------------- |
| v-bind    | 属性绑定。动态地绑定一个或多个 HTML 属性（如 `src`, `href`, `class`, `style`）。                                        |
| v-on      | 事件监听。绑定事件监听器，触发时执行 Vue 实例中的方法。支持事件修饰符（如 `.stop`, `.prevent`）。                       |
| v-model   | 双向数据绑定。主要在表单元素（input, select, textarea）上使用，数据变化视图更新，视图变化数据更新。                     |
| v-if      | 条件渲染 (销毁/重建)。如果表达式为假，元素不会存在于 DOM 中。切换开销大，适合条件不常变动的情况。                       |
| v-else    | 条件渲染 (否则)。配合 `v-if` 使用，表示"否则"的块。                                                                     |
| v-else-if | 条件渲染 (多重判断)。配合 `v-if` 使用，表示"else if"块。                                                                |
| v-show    | 条件渲染 (显示/隐藏)。无论真假，元素始终渲染在 DOM 中，只是切换 CSS 的 `display: none` 属性。切换开销小，适合频繁切换。 |
| v-for     | 列表渲染。基于源数据多次渲染元素或模板块。建议配合 `key` 使用。                                                         |
| v-text    | 更新文本内容。更新元素的 `textContent`。与插值类似，但不会闪烁（FOUC）。                                                |
| v-html    | 更新 HTML 内容。更新元素的 `innerHTML`。注意： 容易导致 XSS 攻击，仅在内容可信时使用。                                  |
| v-pre     | 跳过编译。跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签。                                          |
| v-once    | 只渲染一次。元素和它的所有子节点将被视为静态内容，只渲染一次，后续数据变化不会引起更新。                                |
| v-cloak   | 防止闪烁。这个指令保持在元素上直到关联实例结束编译。配合 CSS `[v-cloak] { display: none }` 解决插值闪烁问题。           |

**语法糖：**  
`v-bind` 指令的语法糖是 `:attr="value"`，用于绑定属性值。  
`v-on` 指令的语法糖是 `@event="handler"`，用于绑定事件监听器。

::: info v-bind 绑定属性

:::

::: info v-on 事件监听

:::

::: info v-if 条件判断

:::

::: info v-for 循环遍历

:::

::: info v-model 双向数据绑定

:::

::: warning ⚠️ 注意

- **v-if vs v-show**
  - `v-if` 是真正的条件渲染（DOM 节点的添加和删除），**惰性**的（如果初始为 false，什么都不会做，直到变 true）。
  - `v-show` 只是 CSS 切换，**不管初始条件如何，元素总是会被渲染**。
- **v-for 的优先级**
  - 当 `v-if` 与 `v-for` 一起使用时，`v-for` 的优先级更高（即：遍历每一项，然后判断是否显示）。**建议：** 尽量避免同时使用，如果为了过滤列表，建议在计算属性中处理。
- **修饰符**
  - `v-on` 和 `v-model` 都有很多修饰符，例如 `@click.stop` (阻止冒泡), `v-model.trim` (去除首尾空格) 等。

:::

## 组件化开发

## 模块化开发

## webpack

## vue cli详解

## vue-router

## vuex详解

## 网络模块封装

## 项目部署
