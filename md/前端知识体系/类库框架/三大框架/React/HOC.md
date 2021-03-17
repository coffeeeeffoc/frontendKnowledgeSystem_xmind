[toc]
# 什么是HOC？
  - 高阶组件，是一个参数为组件，返回值也是一个组件的函数。本身不是组件。
  - 它是一种基于 React 的组合特性而形成的设计模式。
  - 主要用于：
    - 强化组件
    - 复用逻辑
    - 提升渲染性能
## 产生背景
  - 解决横切关注点
## 强化组件的方式
- mixin(已废弃)
  - 特点
  - 缺点
    - 隐式依赖关系,属性不方便跟踪
    - 不同mixins之间可能会有先后顺序甚至代码冲突覆盖的问题
    - 会导致滚雪球式的复杂性
- class mixin
- class extends继承模式
- HOC 模式
- 自定义hooks模式
# HOC种类
- 正向的属性代理
  - 优点
    - 与业务组件解耦，只需条件渲染、属性增强等，无需知道业务逻辑
    - 适用class和function组件
    - 可嵌套使用
    - 相比反向继承，可避免生命周期函数的执行等副作用
  - 缺点
    - 无法直接获取组件的状态，可通过ref获取
    - 无法直接继承静态属性，需手动或者第三方库处理
- 反向的组件继承 
  - 优点
    - 方便获取组件内部状态，比如state，props ,生命周期,绑定的事件函数等
    - es6继承可以良好继承静态属性
  - 缺点
    - 无状态组件无法使用
    - 与被包装组件强耦合，类似mixin的缺点
    - 生命周期函数和状态的覆盖，类似mixin的缺点

# HOC解决问题
- 复用逻辑
- 强化props
  - 劫持props，混入新props
  - 如`react-router`中的`withRouter`
    - 把`history`、`location`、`match`带入到被包装组件的props中
    - 把包装组件props中的`wrappedComponentRef`作为ref传递给被包装组件
    - 包装函数拥有静态属性`WrappedComponent`指向被包装函数
  - 如`react-redux`中`connect`
- 赋能组件
  - 添加新的生命周期，新的事件
- 控制渲染
  - 条件渲染
  - 懒加载渲染
    - 如`dva`中 `dynamic` 组件懒加载
  - 节流渲染
    - 利用useMemo,`useMemo(() =>  <Component {...props}  /> ,[ props.num ])`仅当props.num改变时才重新渲染
  - 分片渲染
# 使用方式
- 装饰器模式
- 函数包裹模式
# HOC使用场景


# 注意点
- 不要在 `render` 方法中使用 `HOC`
  - 会导致每次render时产生新的HOC
- 务必复制静态方法
  - 可以使用 [hoist-non-react-statics](https://github.com/mridgway/hoist-non-react-statics) 自动拷贝所有非 React 静态方法:`hoistNonReactStatic(Enhance, WrappedComponent);`
- Refs 不会被传递
  - 使用`React.forwardRef`
# 参考链接
- https://zh-hans.reactjs.org/docs/higher-order-components.html
- https://juejin.cn/post/6940422320427106335