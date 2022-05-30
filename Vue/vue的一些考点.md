# Vue面试题
## vue面试题
### 1、vue优点
- 轻量级
- 速度快
- 简单易学
- 低耦合
- 可重用性
- 独立开发
- 文档齐全，且文档为中文文档
### 2、vue父子组件传递数据
- props
- $emit
### 3、v-show和v-if指令的共同点和不同点
- 共同点：都是动态显示 DOM 元素
- 区别点：v-if 是动态的向 DOM 树内添加或者删除 DOM 元素
- v-show 是通过设置 DOM 元素的 display 样式属性控制显隐
- v-if 切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件
- v-show 只是简单的基于 css 切换
- 性能消耗
  - v-if 有更高的切换消耗
  - v-show 有更高的初始渲染消耗
- 使用场景
  - v-if 适合运营条件不大可能改变
  - v-show 适合频繁切换
### 4、如何让CSS只在当前组件中起作用
- scoped
### 5、\<keep-alive>\</keep-alive>的作用是什么
- 主要是用于需要频繁切换的组件时进行缓存，不需要重新渲染页面
### 6、如何获取dom
- 给dom元素加ref=‘refname’,然后通过this.$refs.refname进行获取dom元素
### 7、说出几种vue当中的指令和它的用法
- v-model
- v-on
- v-html
- v-text
- v-once
- v-if
- v-show
### 8、vue-loader是什么？它的用途是什么？
- vue文件的一个加载器，将template/js/style转换为js模块
- 用途:js可以写es6、style样式
### 9、为什么用key
- 给每个dom元素加上key作为唯一标识 ，diff算法可以正确的识别这个节点，使页面渲染更加迅速。
### 10、v-on可以监听多个方法吗？
- 可以，比如 v-on=“onclick,onblur”
### 11、$nextTick的使用
- 在data()中的修改后，页面中无法获取data修改后的数据，使用$nextTick时，当data中的数据修改后，可以实时的渲染页面
### 12、vue组件中data为什么必须是一个函数？
- 因为javaScript的特性所导致，在component中，data必须以函数的形式存在，不可以是对象。
- 组件中的data写成一个函数，数据以函数返回值的形式定义，这样每次复用组件的时候，都会返回一份新的data，相当于每个组件实例都有自己私有的数据空间，他们只负责各自维护数据，不会造成混乱。而单纯的写成对象形式，就是所有组件实例共用了一个data，这样改一个全部都会修改。
### 13、vue在双向数据绑定是如何实现的？
- vue双向数据绑定是通过数据劫持、组合、发布订阅模式的方式来实现的，也就是说数据和视图同步，数据发生变化，视图跟着变化，视图变化，数据也随之发生改变
- 核心：关于vue双向数据绑定，其核心是Object.defineProperty()方法vue2.0
### 14、单页面应用和多页面应用区别及缺点
- 单页面应用（SPA），通俗的说就是指只有一个主页面的应用，浏览器一开始就加载所有的js、html、css。所有的页面内容都包含在这个主页面中。但在写的时候，还是分开写，然后再加护的时候有路由程序动态载入，单页面的页面跳转，仅刷新局部资源。多用于pc端。
- 多页面（MPA），就是一个应用中有多个页面，页面跳转时是整页刷新
- 单页面的优点：用户体验好，快，内容的改变不需要重新加载整个页面，基于这一点spa对服务器压力较小；前后端分离，页面效果会比较酷炫
- 单页面缺点：不利于seo；导航不可用，如果一定要导航需要自行实现前进、后退。初次加载时耗时多；页面复杂度提高很多。
### 15、Vue 项目中为什么要在列表组件中写 key，其作用是什么？
- key是给每一个vnode的唯一id,可以依靠key,更准确, 更快的拿到oldVnode中对应的vnode节点。
- 更准确
  - 因为带key就不是就地复用了，在sameNode函数 a.key === b.key对比中可以避免就地复用的情况。所以会更加准确。
  - 更快利用key的唯一性生成map对象来获取对应节点，比遍历方式更快。
### 16、父组件和子组件生命周期钩子执行顺序是什么？
- 加载渲染过程
  - 父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted
- 子组件更新过程
  - 父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated
- 父组件更新过程
  - 父 beforeUpdate -> 父 updated
- 销毁过程
  - 父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed
### 17、谈一谈你对 nextTick 的理解？
- 当你修改了data的值然后马上获取这个dom元素的值，是不能获取到更新后的值，你需要使用$nextTick这个回调，让修改后的data值渲染更新到dom元素之后在获取，才能成功。
### 18、vue更新数组时触发视图更新的方法
- push()；
- pop()；
- shift()；
- unshift()；
- splice()；
- sort()；
- reverse()
### 19、什么是 vue 生命周期？有什么作用？
- 每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做 生命周期钩子 的函数，这给了用户在不同阶段添加自己的代码的机会。
### 20、第一次页面加载会触发哪几个钩子？
- beforeCreate， created， beforeMount， mounted
### 21、vue获取数据在一般在哪个周期函数
- created
- beforeMount
- mounted
### 22、created和mounted的区别
- created:在模板渲染成html前调用，即通常初始化某些属性值，然后再渲染成视图。
- mounted:在模板渲染成html后调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作。
### 23、vue生命周期的理解
- 总共分为8个阶段创建前/后，载入前/后，更新前/后，销毁前/后。
- 创建前/后： 在beforeCreated阶段，vue实例的挂载元素el还没有。
- 载入前/后：在beforeMount阶段，vue实例的$el和data都初始化了，但还是挂载之前为虚拟的dom节点，data.message还未替换。在mounted阶段，vue实例挂载完成，data.message成功渲染。
- 更新前/后：当data变化时，会触发beforeUpdate和updated方法。
- 销毁前/后：在执行destroy方法后，对data的改变不会再触发周期函数，说明此时vue实例已经解除了事件监听以及和dom的绑定，但是dom结构依然存在。
### 24、vuex是什么？vuex有哪几种属性？
- vuex是vue框架中状态管理。
- 有五种，State、 Getter、Mutation 、Action、 Module
- state： 基本数据(数据源存放地)
- getters： 从基本数据派生出来的数据
- mutations ： 提交更改数据的方法，同步！
- actions ： 像一个装饰器，包裹
- mutations，使之可以异步。
- modules ： 模块化Vuex
### 25、v-for 与 v-if 的优先级?
- v-for 和 v-if 同时使用，有一个先后运行的优先级，v-for 比 v-if 优先级更高，这就说明在	v-for 每次的循环赋值中每一次调用 v-if 的判断，所以不推荐 v-if 和 v-for 在同一个标签中同时使用。
### 26、vue 事件中如何使用 event 对象?
- 获取事件对象，方法参数传递 $event 。注意在事件中要使用 $ 符号

```
<button @click="Event($event)">事件对象</button>
```
### 27、组件传值方式有哪些
- 父传子：子组件通过props[‘xx’] 来接收父组件传递的属性 xx 的值
- 子传父：子组件通过 this.$emit(‘fnName’,value) 来传递,父组件通过接收 fnName 事件方法来接收回调
- 其他方式：通过创建一个bus，进行传值
- 使用Vuex
### 28、vue 中子组件调用父组件的方法?
- 直接在子组件中通过 this.parent.event来调用父组件的方法
- 在子组件里面用emit()向父组件触发一个事件
- 父组件监听这个事件就行了。父组件把方法传入子组件中，在子组件里直接调用这个方法。
### 29、vue路由声明式导航router-link跳转
- 不带参数：
- 注意：router-link中链接如果是'/'开始就是从根路由开始，如果开始不带'/'，则从当前路由开始。
```
<router-link :to="{name:'home'}">  
<router-link :to="{path:'/home'}"> //name,path都行, 建议用name 
```

带参数：

  ```
<router-link :to="{name:'home', params: {id:1}}">
<router-link :to="{name:'home', query: {id:1}}">  
<router-link :to="/home/:id">  
  ```

传递对象

```
<router-link :to="{name:'detail', query: {item:JSON.stringify(obj)}}"></router-link> 
```

### 30、Computed和Watch的区别
- computed 计算属性 : 依赖其它属性值,并且 computed 的值有缓存,只有它依赖的 属性值发生改变,下一次获取 computed 的值时才会重新计算 computed 的值。
- watch 侦听器 : 更多的是观察的作用,无缓存性,类似于某些数据的监听回调,每当监听的数据变化时都会执行回调进行后续操作。
- 运用场景：
  - 当我们需要进行数值计算,并且依赖于其它数据时,应该使用 computed,因为可以利用 computed的缓存特性,避免每次获取值时,都要重新计算。
  - 当我们需要在数据变化时执行异步或开销较大的操作时,应该使用 watch,使用 watch 选项允许我们执行异步操作 ( 访问一个 API ),限制我们执行该操作的频率, 并在我们得到最终结果前,设置中间状态。这些都是计算属性无法做到的。
  - 多个因素影响一个显示，用Computed；一个因素的变化影响多个其他因素、显示，用Watch;
  - Computed 和 Methods 的区别
  - computed: 计算属性是基于它们的依赖进行缓存的,只有在它的相关依赖发生改变时才会重新求值对于 method ，只要发生重新渲染
  - method 调用总会执行该函数
### 31、如何解决数据层级结构太深的问题
- 在开发业务时，经常会岀现异步获取数据的情况，有时数据层次比较深，如以下代码: span 'v-text="a.b.c.d">, 可以使用vm.$set手动定义一层数据: vm.$set("demo"，a.b.c.d)

### 32、vue常用指令
- v-model 多用于表单元素实现双向数据绑定（同angular中的ng-model）
- v-bind 动态绑定 作用： 及时对页面的数据进行更改
- v-on:click 给标签绑定函数，可以缩写为@，例如绑定一个点击函数 函数必须写在methods里面
- v-for 格式： v-for=“字段名 in(of) 数组json” 循环数组或json(同angular中的ng-repeat)
- v-show 显示内容 （同angular中的ng-show）v-hide 隐藏内容（同angular中的ng-hide）
- v-if 显示与隐藏 （dom元素的删除添加 同angular中的ng-if 默认值为false）
- v-else-if 必须和v-if连用
- v-else 必须和v-if连用 不能单独使用 否则报错 模板编译错误
- v-text 解析文本v-html 解析html标签
- v-bind:class 三种绑定方法
- 对象型 ‘{red:isred}’
- 三元型 ‘isred?“red”:“blue”’数组型 ‘[{red:“isred”},{blue:“isblue”}]’
- v-once 进入页面时 只渲染一次 不在进行渲染
- v-cloak 防止闪烁
- v-pre 把标签内部的元素原位输出
### 33、route和router的区别
- $route是“路由信息对象”，包括path，params，hash，query，fullPath，matched，name等路由信息参数。
- $router是“路由实例”对象包括了路由的跳转方法，钩子函数等
### 34、怎样理解 Vue 的单项数据流
- 数据总是从父组件传到子组件，子组件没有权利修改父组件传过来的数据，只能请求父组件对原始数据进行修改。这样会防止从子组件意外改变父组件的状态，从而导致你的应用的数据流向难以理解。
- 注意：在子组件直接用 v-model 绑定父组件传过来的 props 这样是不规范的写法，开发环境会报警告。
- 如果实在要改变父组件的 props 值可以再data里面定义一个变量，并用 prop 的值初始化它，之后用$emit 通知父组件去修改。

### 35、Vuex 页面刷新数据丢失怎么解决？
- 需要做 vuex 数据持久化，一般使用本地储存的方案来保存数据，可以自己设计存储方案，也可以使用第三方插件。
- 推荐使用 vuex-persist (脯肉赛斯特)插件，它是为 Vuex 持久化储存而生的一个插件。不需要你手动存取 storage，而是直接将状态保存至 cookie 或者 localStorage中