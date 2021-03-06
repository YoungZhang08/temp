### Vuex是什么？
> Vuex是一个专为Vue.js应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

### 为什么要使用Vuex来做状态管理？
> 1. 多个组件之间共享同一个状态
> 2. Vue组件之间，包括父子、兄弟以及两个没有直接关系的组件之间的通讯

所以，Vuex就将组件的共享状态抽取出来，以一个全局单例模式管理。这个模式下，组件树构成了巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为。

### 什么情况下使用Vuex？
> 一般来说，如果不打算开发大型单页应用，使用global event bus就够了。如果你的应用足够复杂，可能需要考虑如何更好地在组件外部管理状态，Vuex就会成为自然而然的选择。

首先，Vuex的状态存储是响应式的。当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会相应地得到高效更新；其次，你不能直接改变store中的状态，必须通过显式地提交（commit）mutation。

### State
> Vuex使用单一状态树——用一个对象就包含了全部的应用层级状态，也就是作为“唯一数据源”（SSOT）。即每个应用只有一个store实例。

从store实例中读取状态最简单的方法就是在计算属性中返回某个状态。

当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余，可以使用mapState辅助函数帮助生成计算属性，mapState函数返回的是一个对象。

当映射的计算属性的名称与state的子节点名称相同时，我们也可以给mapState传一个字符串数组。

### Getter
> Vuex允许我们在store中定义getter（可以认为是store的计算属性）。就像计算属性一样，getter的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

### Mutation
> 更改Vuex的store中的状态的唯一方法是提交mutation。Vuex中的mutation类似于事件：每个mutation都有一个字符串的事件类型和一个回调函数。这个回调函数就是我们实际进行更改的地方，并且接受state作为第一个参数。

### Action
> Action提交的是mutation，而不是直接变更状态。Action可以包含任意异步操作。