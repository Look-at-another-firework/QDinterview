# Vue的学习

## 写Vue项目时为什么要在列表组件中写key，其作用是什么?

简单的说：

一、不使用key：

>就地复用节点。
>
>无法维持组件的状态。
>
>性能下降。

二、使用key：

>维持组件的状态，保证组件的复用。
>
>查找性能上的提升。
>
>节点复用带来的性能提升。

总结：性能提升不能只考虑一方面，不是diff 快了性能就快，不是增删节点少了性能就快，不考虑量级的去评价性能，都只是泛泛而谈。

完整解读：

> 为了给Vue —个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一ey 属性。理想的key值是每项都有唯一id。
>
> 建议尽可能在使用v-for时提供key attribute，除非遍历输出的DOM内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
>
> 因为它是Vue识别节点的一个通用机制，key并不与v-for特别关联，key还具有其他用途
>
> key的特殊属性主要用在Vue的虚拟DOM算法，在新旧nodes 对比时辨识VNodes。如果不使用key,Vue 会使用一种最大限度减少动态元素并且尽可能的尝试修复/再利用相同类型元素的算法。使用key，它会基于key的变化重新排列元素顺序，并且会移除key 不存在的元素。
>
> 有相同父元素的子元素必须有独特的key。重复的key会造成渲染错误。

## 聊聊Vue的双向数据绑定，Model 如何改变View，View又是如何改变Model的

该篇幅内容较大，覆盖了`vue2`和`vue3`的数据绑定，建议去查看本博主的文章，地址：<[vue3的学习 | Welcome (letter.gx.cn)](https://www.letter.gx.cn/posts/30.html)>，加载缓慢可以参考：<[( vue3的学习)](https://blog.csdn.net/The_more_more/article/details/124655585)>

## 在Vue中，子组件为何不可以修改父组件传递的 Prop。

简单来说就是：为了保证数据的单向流动，便于对数据进行追踪，避免数据混乱。

`vue`数据传递是单项数据流，如果父组件传递过米的props为`引用类型`，直接修改了`props`的值会导致其他用到此`props`的子组件的值也发生改变，导致数据混乱，以及难以追踪的bug。如果props是`基础类型`，不会造成其他影响，但往享会是整个组件`数据混乱`。

## Vue的父组件和子组件生命周期钩子执行顺序是什么

加载渲染过程

> 父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted

子组件更新过程

> 父beforeUpdate->子beforeUpdate->子updated->父updated

父组件更新过程

> 父beforeUpdate->父updated

销毁过程

> 父beforeDestroy->子beforeDestroy->子destroyed->父destroyed

##  vue在v-for时给每项元素绑定事件需要用事件代理吗?为什么?

事件代理的作用：

1. 事件代理能够避免我们逐个的去给元素新增和删除事件
2. 事件代理比每一个元素都绑定一个事件性能要更好

从vue的角度来看问题：

1. 在`v-for`中，我们直接用一个for循环就能在模板中将每个元素都绑定上事件，并且当组件销毁时，vue也会自动给我们将所有的事件处理器都移除掉。所以事件代理能做到的第一点vue已经给我们做到了
2. 在`v-for`中，给元素绑定的都是相同的事件，所以除非上千行的元素需要加上事件，其实和使用事件代理的性能差别不大，所以也没必要用事件代理

## vue 渲染大量数据时应该怎么优化?

建议：

1. 异步渲染组件
2. 使用`vue`的`v-if`最多显示上中(可视区域)下三屏避免出现大量的`dom`节点
3. 或者使用分页
4. 添加加载动画，优化用户体验
5. 利用服务器渲染`SSR`，在服务端渲染组件
6. 避免浏览器处理大量的`dom`，比如懒加载，异步渲染组件，使用分页
7. 对于固定的非响应式的数据，使用`Object.freeze`冻结

## Vue 中的computed是如何实现的

1. `computed`本身是通过代理的方式代理到组件实例上的，所以读取计算属性的时候，执行的是一个内部的`getter`，而不是用户定义的方法。
2. `computed`内部实现了一个惰性的`watcher`，在实例化的时候不会去求值，其内部通过`dirty`属性标记计算属性是否需要重新求值。当`computed`依赖的任—状态(不一定是return中的)发生变化，都会通知这个惰性`watcher`，让它把`dirty`属性设置为`true`。所以，当再次读取这个计算属性的时候，就会重新去求值。

##  Vue中的computed和watch的区别在哪里

1. `computed`:计算属性

> 计算属性是由data中的已知值，得到的一个新值。
> 这个新值只会根据已知值的变化而变化，其他不相关的数据的变化不会影响该新值。
> 计算属性不在data中，计算属性新值的相关已知值在data中。
> 别人变化影响我自己。

2. `watch`:监听数据的变化

> 监听data中数据的变化
> 监听的数据就是data中的已知值我的变化影响别人

区别：

1. `watch`擅长处理的场景:一个数据影响多个数据

2. `computed`擅长处理的场景:—个数据受多个数据影响

##  v-if、v-show、v-html的原理是什么，它是如何封装的?

1. `v-if`会调用`addIfConditio`n方法，生成`vnode`的时候会忽略对应节点,`render`的时候就不会渲染;
2. `v-show`会生成`vnode`,`render`的时候也会渲染成真实节点，只是在`render`过程中会在节点的属性中修改show属性值，也就是常说的`display`;
3. `v-html`会先移除节点下的所有节点，调用`html`方法，通过`addProp`添加`innerHTML`属性，归根结底还是设置`innerHTML`为`v-html`的值。

## 什么是MVVM

1. `MVVM`是`Model-View-ViewModel`缩写，也就是把`MVC`中的`Controller`演变成`ViewModel`。`Model`层代表数据模型，
2. `View`代表`UI组件`，`ViewModel`是`View`和`Model`层的桥梁，数据会绑定到`viewModel`层并自动将数据渲染到页面中，
3. 视图变化的时候会通知`viewModel`层更新数据。

优点：

1. 低耦合，视图（`View`）可以独立于`Model`变化和修改，一个`ViewModel`可以绑定到不同的”`View`”上，当View变化的时候`Model`可以不变，当`Model`变化的时候`View`也可以不变。
2. 可重用性，可以把一些视图逻辑放在一个`ViewModel`里面，让很多`view`重用这段视图逻辑。
3. 独立开发，开发人员可以专注于业务逻辑和数据的开发（`ViewModel`），设计人员可以专注于页面设计，使用`Expression Blend`可以很容易设计界面并生成xml代码。
4. 可测试，界面向来是比较难于测试的，而现在测试可以针对`ViewModel`来写。

## vue的组件通信的几种方式

1. `props `和`$emi`t 父组件向子组件传递数据是通过 `prop` 传递的，子组件传递数据给父组件是通过`$emit` 触发事件来做到的
2. `$parent`,`$children` 获取当前组件的父组件和当前组件的子组件
3. `$attrs` 和`$listeners` A->B->C。Vue 2.4 开始提供了$attrs 和$listeners 来解决这个问题
   (如果一个第三方的组件有100个props和100个$emit触发的事件，你在对它进行二次封装的时候如果需要支持其所有的props和$emit触发的事件，你就可以用$attr和$listeners轻松解决，否则的话你还得写100个props和100个$emit触发的事件。还有多级组件嵌套的情况也很适用，不用将props一层一层往上传递。)
4. 父组件中通过 `provide` 来提供变量，然后在子组件中通过` inject `来注入变量。(官方不推荐在实际业务中使用，但是写组件库时很常用)
5. `$refs` 获取组件实例
6. `envetBus` 兄弟组件数据传递 这种情况下可以使用事件总线的方式
7. `vuex` 状态管理
8. `locastorage` 和 `sessionStorage`
