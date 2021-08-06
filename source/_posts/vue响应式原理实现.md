---
title: vue响应式原理实现
date: 2020-08-30 17:15:37
tags:
    - javaScript
    - vue
---

实现 Vue 数据响应式 （数据劫持结合发布者-订阅者）、数组的变异方法、编译指令，数据的双向绑定的功能。
<!-- more -->

#### 前言

##### 什么是 MVVM 思想？

MVVM 是 Model-View-ViewModel，是把一个系统分为了模型（ model ）、视图（ view ）和 view-model 三个部分。

Vue在 MVVM 思想下，view 和model 之间没有直接的联系，但是 view 和 view-model 、model和 view-model之间是交互的，当 view 视图进行 dom 操作等使数据发生变化时，可以通过 view-model 同步到 model 中，同样的 model 数据变化也会同步到 view 中。

##### 实现数据响应式都有那些方法？

1、**发布者-订阅者**：当一个对象（发布者）状态发生改变时，所有依赖它的对象（订阅者）都会得到通知。

2、**脏值检查**：储存旧数据，和当前新的数据进行对比，根据数据是否有变动来决定是否更新数据（会增加性能消耗）。

3、**数据劫持**：通过 Object.defineProperty() 劫持各个属性的getter，setter，在数据变动时，触发相应的方法。

##### Vue是如何实现响应式的呢？

Vue是通过数据劫持，结合发布者-订阅者模式，来实现数据响应式的。

当执行 new Vue() 时，VUE 就进入了初始化阶段，Vue会对指令进行解析（初始化视图，增加订阅者，绑定更新函数），同时通过 Observer 遍历数据并通过 Object.defineProperty 的 getter 和 setter 实现对数据的监听， 当数据发生变化的时候，Observer 中的 setter 方法被触发，setter 会立即调用Dep.notify()， Dep 开始遍历所有的订阅者，并调用订阅者的 update 方法，订阅者收到通知后对视图进行相应的更新。

1、**Obserber**：数据监听器，能够对数据对象的所有属性进行监听，如有变动可拿到最新值并通知订阅者，内部采用 Object.defineProperty 的 getter 和 setter 来实现。

2、**Compile**：指令解析器，它的作用对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数。

3、**Dep**：订阅者收集器或者叫消息订阅器都可以，它在内部维护了一个数组，用来收集订阅者，当数据改变触发 notify 函数，再调用订阅者的 update 方法。

4、**Watcher**：订阅者，它是连接 Observer 和  Compile 的桥梁，收到消息订阅器的通知，更新视图。

5、**Updater**：视图更新。

所以我们想要实现一个 Vue 响应式，需要完成数据劫持、依赖收集、发布者-订阅者模式。

##### 我将要实现Vue的功能：

1、数据响应。

2、Vue常用指令，v-html, v-text, v-bind, v-on, v-model。

3、数组变异方法的处理。

4、实现数据代理，在Vue中使用this访问并改变data数据。

##### 要实现上面的功能，我们需要实现几个类的方法

1、**Observer类**：实现对所有数据进行监听。

2、**array工具类**：对变异方法的处理。

3、**Dep类**：维护订阅者。

4、**Watcher**：接受 Dep 的更新通知，更新视图。

5、**Compile**：对指令进行解析。

6、**CompileUtils**：实现通过指令更新视图，绑定更新函数 Watcher。

7、实现 this.data 代理。

##### 开始之前，我们先使用 npm, webpack 来初始化和管理一个项目

1、使用npm包管理工具初始化，按照提示进行操作。

```npm init```

2、安装 webpack 及相关依赖。

```
npm install clean-webpack-plugin --save-dev // 自动清空dist目录
npm install html-webpack-plugin --save-dev  // html处理工具
npm install webpack --save-dev  // webpack
npm install webpack-cli --save-dev  // webpack
npm install webpack-dev-server --save-dev // webpack express 服务
npm install css-loader style-loader --save-dev // css加载器
```

3、创建 webpack.config.js。

```
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
module.exports = {
    mode: 'development',
    devtool: 'sourcemap',
    entry: './public/index.js',  //entry:入口文件
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html' //可以指定模板html，也可以使用默认的
        }),
        new CleanWebpackPlugin(),
    ],
    module:{
        rules:[
          {test:/\.css$/ , use:['style-loader' , 'css-loader']}
        ]
    }
}
```

4、创建 public 文件夹及 index.html 和 index.js 入口文件。

5、在 package.json 中添加script语句，来启动 express 服务。

````
"serve": "webpack-dev-server" // 添加 --open 参数可自动打开浏览器
````

6、运行服务。

```
yarn serve
```

##### 实现 Observer 类

要用 Obeject.defineProperty() 来监听属性的数据变化，我们需要对 Observer 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter ，这样的话，当给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化。当然我们在新增加数据的时候，也要对新的数据对象进行递归遍历，加上 setter 和 getter 。

但我们要注意数组，在处理数组时并不是把数组中的每一个元素都加上 setter 和 getter ，我们试想一下，一个从后端返回的数组数据是非常庞大的，如果为每个属性都加上 setter 和 getter ，性能消耗是十分巨大的。我们想要得到的效果和所消耗的性能不成正比，所以在数组方面，我们通过对数组的7 个变异方法来实现数据的响应式。只有通过数组变异方法来修改和删除数组时才会重新渲染页面。
那么监听到变化之后是如何通知订阅者来更新视图的呢？我们需要实现一个Dep（消息订阅器），其中有一个 notify() 方法，是通知订阅者数据发生了变化，再让订阅者来更新视图。

我们怎么添加订阅者呢？我们可以通过 new Dep()，通过 Dep 中的addSubs() 方法来添加订阅者。我们来看一下具体代码。

我们首先需要声明一个 Observer 类，在创建类的时候，我们需要创建一个消息订阅器，判断一下是否是数组，如果是数组，我们便改造数组，如果是对象，我们便需要为对象的每一个属性都加入 setter 和 getter 。

1、创建 Vue.js
```
import Observer from 'Observer.js'
export default class Vue {
    constructor (options) {
        this.$el = options.el
        // 获取data中的数据，并进行数据劫持
        this.$data = options.data
        new Observer(this.$data) // 引入 Observer 类
    }
}
```

2、创建 Observer 类
```

```



















