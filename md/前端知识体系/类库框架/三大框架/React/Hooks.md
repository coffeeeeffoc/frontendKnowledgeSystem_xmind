# hooks
## hooks解决的问题
  - 函数组件中不能拥有自己的状态
    - hooks用useState维护状态
  - 函数组件中不能监听组件的生命周期
    - 几种生命周期可以用hooks模拟实现
  - class组件中生命周期较为复杂
  - class组件逻辑难以复用
    - class采用HOC和render props来逻辑复用
## hook相对class的优势
  - 写法更加的简洁
  - 业务代码更加聚合
    - 挂载和卸载时处理逻辑不需要在两个函数中写
  - 逻辑复用方便
    - 自定义hooks
  
# API
## useState
### 实现原理
  利用闭包，存了一个数组来保存函数的状态，多次调用useState时，按顺序依次调用