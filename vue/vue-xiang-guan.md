# Vue相关

## vue

### MVVM\(Model-View-ViewModel\)模型

`MVVM`分为`Model`、`View`、`ViewModel`三部分。

* `Model`代表数据模型，定义数据和业务逻辑，访问数据层
* `View`代表视图，展示页面结构、布局和外观（UI）
* `ViewModel`代表视图模型，负责监听`Model`数据变化并更新视图，处理用户交互 `Model`和`View`是通过`ViewModel`，`Model`的数据变化会触发`View`的更新，`View`的交互操作也会使`Model`的数据发生改变。只需要针对数据进行维护操作，数据的自动同部不需要通过操作`dom`实现。

### Vue指令

#### 内置指令

指令的本质就是语法糖或者标志位。

| 指令 | 作用 | 期望数值类型 |
| :--- | :--- | :--- |
| v-text | 更新元素文本内容 | string |
| v-html | 更新元素的`innerHTML`，不推荐使用 | string |
| v-show | 条件渲染。根据表达式的真假值，控制元素的显示或隐藏 | any |
| v-if | 条件渲染。根据表达式的值的真假条件选择是否渲染元素这个节点 | any |
| v-else | 条件渲染。根据`v-if`的相反条件进行元素渲染 | any |
| v-else-if | 条件渲染。做`v-if`的链式调用 | any |
| v-for | 列表渲染。对数据进行遍历渲染，最好提供`key`值 | Array / Object / number / string |
| v-on | 事件处理。绑定事件监听器，事件类型由参数指定，表达式可以是方法名或内联语句。 | Function / Inline Statement / Object |
| v-bind | 动态绑定。动态绑定一个或多个特性，或一个组件`prop`到表达式 | any \(with argument\) / Object \(without argument\) |
| v-model | 表单绑定。在表单或组件是上创建双向绑定 | 随表单控件类型变化 |
| v-pre | 跳过该元素和它的子元素的编译过程，直接输出模板字符串 |  |
| v-slot | 作用域插槽 |  |
| v-cloak | 设置 `[v-cloak] { display: none }`可以在渲染时延后加载`Vue`实例，避免闪现 |  |
| v-once | 元素和组件只渲染一次，重新渲染,元素/组件及其所有的子节点将被视为静态内容并跳过 |  |

#### 自定义指令

不是刚需，和生命周期有很大关系，可见五个生命周期钩子。

* `bind`
* `inserted`
* `update`
* `componentUpdated`
* `unbind`

返回顶部

### Vue响应式原理

![](https://vue.docschina.org/images/data.png) 

`Vue`实例化时，遍历访问`data`里的所有属性，使用`Object.defineProperty`将其属性全部转换为`getter/setter`进行依赖追踪以便修改属性时进行变更通知，就是一个代理层，不管是获取数据还是什么，都是在代理层里进行，当组件渲染时，会从代理层进行代理映射，组件渲染需要什么就会放在`watcher`中，因为每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 `setter` 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新，没有与之关联的组件就不会更新。

### Vue双向数据绑定和单项数据流

**什么是双向数据绑定**

`model`的更新会触发`view`的更新，`view`的更新也会触发`model`的更新

**什么是单向数据流**

`model`的更新会触发`view`的更新，但是`view`的更新不会触发`model`的更新

**双向绑定 or 单向数据流**

* `Vue`是单向数据流，不是双向绑定
* `Vue`的双向绑定不过是语法糖
* `Object.defineProperty`是用来做响应式更新的，和双向绑定没关系

返回顶部

**简单实现一个响应式双向数据绑定**

简单实现，有一个子组件输入框，一个按钮，父组件通过`props`传值给子组件，当按钮增加时，子组件通过`$emit`通知父组件修改相应的`props`值。

```text
<div id="app">
        <parent></parent>
    </div>
    <script>
        var childNode = {
            template:`
                <div class = "child">
                    <div>
                        <span>子组件数据</span>
                        <input v-model="childMsg">
                        <button @click=add>+1</button>
                    </div>
                </div>
            `,
            data(){
                return{
                    childMsg:0
                }
            },
            methods: {
                add(){
                    this.childMsg++;
                    this.$emit('update:foo',this.childMsg)
                }
            }
        };
        var parentNode = {
            template:`
                <div class="parent">
                    <div>
                        <span>父组件数据:{{msg}}</span>
                        <child :foo.sync="msg"></child>
                    </div>
                </div>
            `,
            components:{
                'child':childNode
            },
            data(){
                return{
                    'msg':0
                }
            }
        };
        let vm = new Vue({
            el:'#app',
            components:{
                'parent':parentNode
            }
        })
```

 返回顶部

### 合理应用计算属性和侦听器

#### 计算属性（`computed`）的应用

* 处理数据计算，减少模板中计算逻辑
* 数据缓存。当计算的数据比较多的时候，放在计算属性中，不会在每次渲染界面的重新计算，提高页面性能
* 它必须依赖固定的数据类型（响应式数据），而不是全局数据

#### 侦听器`watch`

* 更加灵活、通用,可以触发一系列的操作
* `watch`提供一个底层`API`,可以执行任何逻辑，如函数节流、`Ajax`异步获取数据，操作`DOM`，但是不推荐

#### 计算属性和侦听器的应用场景

* 计算属性`computed`能做的,侦听器`watch`都能做，反之不行
* 能用计算属性`computed`的尽量不用侦听器`watch`

### Vue的生命周期

生命周期就是实例从创建到销毁的过程，生命周期一共分为三个阶段，创建阶段（执行一次）、更新阶段（执行多次）、销毁阶段，控制整个Vue实例过程形成好的结构。

生命周期一共八个阶段：创建前/后、挂载前/后、更新前/后、销毁前/后

![](https://vue.docschina.org/images/lifecycle.png)

 返回顶部

#### 生命周期的应用场景

| 钩子 | 调用 | 类型 | 是否在服务端渲染期间调用 |
| :--- | :--- | :--- | :--- |
| beforeCreate | `Vue`实例初始化之后，数据观察和事件配置之前 | Function | Yes |
| create | 实例创建完成（数据观察/属性和方法运算/侦听器配置/事件回调）之后，挂载之前 | Function | Yes |
| beforeMount | 挂载开始之前，模板`render`函数首次调用 | Function | Yes |
| mounted | 实例挂载完成之后，异步请求/操作`DOM`/定时器 | Function | No |
| beforeUpdate | `DOM`被`patch`之前调用进行数据修改，可以移除已添加的事件监听器等，不可更改依赖数据 | Function | No |
| updated | 组件 DOM 更新完成，避免在此期间更改状态（依赖数据） | Function | No |
| actived | keep-alive 组件激活时 | Function | No |
| deactived | keep-alive 组件停用时 | Function | No |
| beforeDestroy | 实例销毁之前，可以移除已添加的事件监听器等 | Function | No |
| destroyed | 实例销毁 | Function | No |
| errorCaptured | 当任何一个来自后代组件的错误时被捕获时 收到三个参数：错误对象、发生错误的组件实例，和一个包含错误在何处被捕获信息的字符串 返回 false，以阻止该错误继续向上冒泡 | Function | No |

#### 函数式挂件

一般用于做展示用。

* `functional:true`
* 无状态、无实例、没有`this`上下文、无生命周期

返回顶部

### keep-alive

`keep-alive`是内置组件，用于使被包含的组件保留状态，避免重新渲染。

* 一般组合路由和动态组件使用，用于缓存组件
* 提供`include`和`exclude`属性，支持字符串或正则表达式， include 表示只有名称匹配的组件会被缓存，exclude 表示任何名称匹配的组件都不会被缓存 ，其中 exclude 的优先级比 include 高；
* 对应两个钩子函数 activated 和 deactivated ，当组件被激活时，触发钩子函数 activated，当组件被移除时，触发钩子函数 deactivated

### nextTick\(\)

在修改数据之后，立即使用的这个回调函数，获取更新后的`DOM`

### vue插槽

#### 单个插槽

当子组件模板只有一个没有属性的插槽时， 父组件传入的整个内容片段将插入到插槽所在的 DOM 位置， 并替换掉插槽标签本身

#### 具名插槽

`solt`元素可以用一个特殊的特性`name`来进一步配置如何分发内容。 多个插槽可以有不同的名字，这样可以将父组件模板中 `slot`位置， 和子组件`slot`元素产生关联，便于插槽内容对应传递。

#### 作用域插槽

可以访问组件内部数据的可复用插槽在父级中，具有特殊特性 `slot-scope`的&lt;template&gt; 元素必须存在， 表示它是作用域插槽的模板。`slot-scope` 的值将被用作一个临时变量名， 此变量接收从子组件传递过来的 `prop`对象

作者：小明同学哟 链接：[https://juejin.im/post/5d87985d6fb9a06add4e6ac3](https://juejin.im/post/5d87985d6fb9a06add4e6ac3) 来源：掘金 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### Vue组件

![](https://vue.docschina.org/images/components.png)

`Vue`组件 = `Vue`实例 = `new Vue(options)`，每个组件就是一个`Vue`实例 组件可以是页面中每一个区域板块，也可以是某一个复用业务逻辑，也可以是每一个单页面。

#### 组件的构成

 就以上面的双向数据绑定实现为例：

* 属性：
  * 自定义属性`props`：组件`props`中声明的属性，父组件使用`props`定义数据属性，向子组件传递数据
  * 原生属性`attrs`：没有声明的属性，默认自动挂载到组件根元素上，设置`inheritAttrs`为`false`可以关闭自动挂载
  * 特殊属性`class`、`style`：挂载在组件根元素上，支持字符串、对象、数组等多种语法
* 事件`event`：
  * 普通事件：`@click` 、`@input`、`@change`、`@xxx`等事件，通过`this.$emit('xxx',...)`触发自定义事件`event`向父组件发送消息
  * 修饰符事件：`@input.trim`、`@click.stop`、`@submit.prevent`等，一般用于原生`HTML`元素，自定义组件需要自行开发支持
* 插槽`slot`：
  * 普通插槽：`slot`进行组件内容分发，插入子组件内容，简单点就是传递内容的 `<template slot="xxx">...</template>`、`<template v-slot="xxx">...</template>`
  * 作用域插槽：需要根据子组件的某些值来做动态处理，可以简单理解为返回组件的函数 `<template slot="xxx" slot-scope="props">...</template>`、`<template v-slot:xxx="props">...</template>`

#### 组件通信

* 父子组件通信：父组件使用`props`向子组件通信，子组件使用`$emit`向父组件传递消息
* 非父子组件通信：通过中央事件总线的监听和触发来实现通信和传递
* 跨级组件通信：使用`Vuex`比较好管理

**如何优雅地获取跨层级组件实例（拒绝递归）**

```text
<p ref="p">hello</p>
<child-component ref="child></child-component>
```

**callback ref**

* 主动通知（setXxxRef）
* 主动获取（getXxxRef）

返回顶部

#### 组件更新

组件的更新都是由数据驱动的，没有特殊情况，任何更改`DOM`的行为都是在作死。

**数据来源（单向）**

包含三个部分：

* 来自父组件的属性`props`
* 来自组件自身的状态`data`
* 来自状态管理器，如`vuex vue.observable`

**注意**

状态和属性的改变未必会触发组件更新

### 高级特性provide/inject

一般用于底层组件通信。底层组件通信，不仅属性要层层传递，事件也要层次冒泡，这是很耗性能的。

### Vue-router路由

 Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：

* 嵌套的路由/视图表
* 模块化的、基于组件的路由配置
* 路由参数、查询、通配符
* 基于 Vue.js 过渡系统的视图过渡效果
* 导航控制
* 带有自动激活的 CSS class 的链接
* HTML5 历史模式或 hash 模式，在 IE9 中自动降级
* 自定义的滚动条行为

#### 路由的导航钩子

* 全局导航钩子：`router.beforeEach(to,from,next)`，跳转前进行判断拦截
* 组件内钩子
* 单独路由独享组件

下面是一个简单路由的实现：

```text
 <div id="app" class="demo">
       <h1>Hello App!</h1>
       <p>
           <!-- 通过router-link导航 -->
           <!-- 通过传入'to '属性指定链接-->
           <!--  <router-link> 默认会被渲染成一个 `<a>` 标签-->
               <router-link to="/foo">go to Foo</router-link>
               <router-link to="/bar">go to Bar</router-link>
       </p>
       <!-- 路由出口 -->
       <!-- 路由匹配到的组件将渲染在这里 -->
       <router-view></router-view>
    </div>
    <!-- 
        0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)
        1. 定义 (路由) 组件。
        2.定义路由
     -->
    <script>
        //1. 定义 (路由) 组件。
        const Foo = {template:'<div>foo</div>'};
        const Bar = {template:'<div>bar</div>'};
        //2.定义路由
        //每个路由应该映射一个组件。其中component可以是通过Vue.extend()创建的组件构造器
        //或者只是一个组件配置对象
        const routes = [
            {path:'/foo',component:Foo},
            {path:'/bar',component:Bar}
        ]
​
        //3.创建router实例，然后传‘routes’配置
        const router = new VueRouter({
            routes//（缩写）相当于routes:routes
        })
​
        //4.创建和挂载根实例
        //要记得通过router配置参数注入路由，从而让整个应用都有路由功能
        
        const app = new Vue({
            router
        }).$mount('#app');
```

 返回顶部

### Vuex

`Vuex`就相当于一个仓库，是**状态管理模式**，存放多个实例间共享的状态，来实现数据共享。

`vuex`更多地用于解决跨组件通信以及作为数据中心集中式存储数据

`Vuex`使用单一状态树,即每个应用将仅仅包含一个`store`实例，但单一状态树和模块化并不冲突。

* `state`：存放响应式状态，不可以直接更改其数据
* `getter`：类似计算属性，用于从`store`中的 `state`中派生出一些状态，比如过滤一些数据
* `mutation`：更改 Vuex 的 store 中的状态的唯一方法是提交 mutation，**mutation 必须是同步函数**
* `action`：`action`类似于 `mutation`，但是`action` 提交的是 `mutation`，而不是直接变更状态，包含任意异步操作。`view` 层通过`store.dispath`来分发`action`

### Vue项目中进行的优化

* `v-if`和`v-show`根据场景使用：频繁切换用v-show
* `v-for`遍历必须为`item`添加`key`
* 长列表性能优化
* 事件销毁
* 图片懒加载
* 插件按需引入
* 路由懒加载
* 服务端`SSR`渲染

