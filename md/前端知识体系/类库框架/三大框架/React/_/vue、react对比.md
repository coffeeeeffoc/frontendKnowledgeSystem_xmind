# vue、react对比

## 相同点

### 使用 Virtual DOM，有自己的diff渲染算法

### 提供了响应式 (Reactive) 和组件化 (Composable) 的视图组件。

### 将注意力集中保持在核心库，而将其他功能如路由和全局状态管理交给相关的库。

## 不同点

### 监听数据变化的实现原理不同

- Vue 通过 getter/setter 以及一些函数的劫持，能精确知道数据变化，不需要特别的优化就能达到很好的性能
- React 默认是通过比较引用的方式进行的，如果不优化（PureComponent/shouldComponentUpdate）可能导致大量不必要的VDOM的重新渲染


- 为什么 React 不精确监听数据变化呢？这是因为 Vue 和 React 设计理念上的区别，Vue 使用的是可变数据，而React更强调数据的不可变。所以应该说没有好坏之分，Vue更加简单，而React构建大型应用的时候更加鲁棒。

### 数据流的不同

- vue1.0双向绑定方式

	- 父子组件之间，props 可以双向绑定
	- 组件与DOM之间可以通过 v-model 双向绑定

- react单向数据流

### HoC 和 mixins

- 在 Vue 中我们组合不同功能的方式是通过 mixin，而在React中我们通过 HoC (高阶组件）

### 组件通信的区别

- vue

	- 父组件通过 props 向子组件传递数据或者回调，虽然可以传递回调，但是我们一般只传数据，而通过 事件的机制来处理子组件向父组件的通信
	- 子组件通过 事件 向父组件发送消息
	- 通过 V2.2.0 中新增的 provide/inject 来实现父组件向子组件注入数据，可以跨越多个层级。

- react

	- 父组件通过 props 可以向子组件传递数据或者回调
	- 可以通过 context 进行跨层级的通信，这其实和 provide/inject 起到的作用差不多。

### 模板渲染方式的不同

- 表层-语法不同

	- Vue是通过一种拓展的HTML语法进行渲染
	- React 是通过JSX渲染模板

		- React并不必须依赖JSX。

- 深层-原理不同

	- Vue是在和组件JS代码分离的单独的模板中，通过指令来实现的，比如条件语句就需要 v-if 来实现
	- React是在组件JS代码中，通过原生JS实现模板中的常见语法，比如插值，条件，循环等，都是通过JS语法实现的

### Vuex 和 Redux 的区别

- 表面上来说，store 注入和使用方式有一些区别

	- vuex

		- 使用 dispatch 和 commit 提交更新

通过 mapState 或者直接通过 this.$store 来读取数据

	- Redux

		- 用 connect 把需要的 props 和 dispatch 连接起来
		- 只能进行 dispatch修改

- 深层-实现原理不同

	- Redux 使用的是不可变数据，而Vuex的数据是可变的。Redux每次都是用新的state替换旧的state，而Vuex是直接修改
	- Redux 在检测数据变化的时候，是通过 diff 的方式比较差异的，而Vuex其实和Vue的原理一样，是通过 getter/setter来比较的（如果看Vuex源码会知道，其实他内部直接创建一个Vue实例用来跟踪数据变化）

## 各自优势

### Vue的优势

- 模板和渲染函数的弹性选择
- 简单的语法及项目创建
- 更快的渲染速度和更小的体积

### React的优势

- 更适用于大型应用和更好的可测试性
- 同时适用于Web端和原生App
- 更大的生态圈带来的更多支持和工具

