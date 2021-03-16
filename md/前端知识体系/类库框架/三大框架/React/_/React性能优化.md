# React性能优化

## 利用工具

### React Perf

## 单组件性能优化

### shouldComponentUpdate

- return true 表示需要重新渲染
return false 表示无需渲染

### react-redux库的connect方法，已经封装了shouldComponentUpdate的逻辑

- 所以如果不需要读取状态或者派发动作，则可以单纯调用connect方法，仅仅为了利用shouldComponentUpdate
- 例子：
export defaul connect()(TodoItem);

### jsx中，尽量不要直接使用新对象，而是使用定义过的变量或者对象

- 每次都是新的对象，浅层比较对象是否相等，当然会不一样

## 多组件的性能优化

### react的调和过程（Reconciliation）

- virtualDOM比较算法复杂度是O(N)
- 节点类型是否相同

	- 不同则卸载、重装
	- 相同则继续，判断节点是DOM类型，还是React定制组件

		- DOM节点

			- 判断属性和内容，如不一致则修改

		- 定制类型

			- 依次出发生命周期函数

				- componentWillReceiveProps
				- shouldComponentUpdate
				- componentWillUpdate
				- render
				- componentDidUpdate

	- 要用key属性，避免重复比较

### 注：key和ref是React保留的两个特殊的prop

## 用reselect提高数据获取性能

### 两段选择过程

- 1.从state获取第一层结果，如果比较之后发现完全相同，则把之前的缓存的结果返回
如果结果不同，则执行下一步
- 2.依次比较计算state的异同

### 范式化状态树

- 遵照关系型数据库的设计原则，减少冗余数据

	- 由于有步骤一，会读取缓存，所以读取效率没有那么低

- 与之相对，反范式化，代表是Nosql领域。利用数据冗余换取读取的效率

