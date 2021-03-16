# React组件

## 组件的设计要素

### 高内聚

### 低耦合

## 组件的数据

### propTypes检查
（propTypes属性）

- 定义prop的规格

	- 1.该组件支持哪些prop
	- 2.每个prop应该是什么格式

- 例子
Counter.propTypes = {
  caption: PropType.string.isRequired,
  initValue: PropType.number
}

### defaultProps

- this.state中值的初始值
- 例子
Counter.defaultProps= {
  caption: ‘’,
  initValue: 0
}

### forceUpdate

- 每个组件都可以通过forceUpdate函数强制引发一次重新绘制

### prop和state的对比

- 1.prop定义外部接口、state用于记录内部状态
- 2.prop的赋值在外部、state的赋值在组件内部
- 3.组件不应该改变prop的值、state存在目的就是为了让组件内来改变值

## 组件的生命周期

### 装载--Mount

- constructor

	- 初始化state
	- 绑定成员函数的this

- getInitialState
- getDefaultProps
- componentWillMount
- render

	- 最重要的函数
	- 应该是一个纯函数，完全根据this.state和this.props来决定返回结果，不要产生任何副作用
	- 如果是特殊组件，不需要渲染界面，则返回false或者null

- componentDidMount

	- 如果要与UI库或者jQuery结合的话，就在此函数中操作
	- 原因

		- 所有的DOM已经存在

### 更新--Update

- componentWillReceiveProps(nextProps)

	- this.setState方法触发的更新过程不会执行此函数
	- 在props发生改变的时候更新，
	- 只要父组件调用render函数，就会触发此函数
	- 多组件情况

		- parent render
		- children1 componentWillReceiveProps
		- children1 render
		- children2 componentWillReceiveProps
		- children2 render
		- children3 componentWillReceiveProps
		- children4 render

- shouldComponentUpdate(nextProps, nextState)

	- 生命周期中第二重要的函数
	- render函数重要，是因为render函数决定了该渲染什么。

shouldComponentUpdate(nextProps, nextState)重要，是因为决定了一个组件什么时候不需要渲染

		- 唯二两个要求 又返回结果的函数

	- 返回结果

		- 返回true，继续更新
		- 返回false，立刻停止更新

- componentWillUpdate
- render
- componentDidUpdate

	- 无论更新过程发生在客户端或者服务端，componentDidUpdate函数都会被调用，只不过此函数不会再服务端被调用
	- 需要再次调用jQuery代码

### 卸载--Unmount

- componentWillUnmount

## 向外传递数据

### 父组件传入一个props的回调函数，子组件执行此函数

## state和prop的局限

### 层层传递props，中间没必要的传递太多

### state存储数据的缺点

- 数据的冗余
- 发生冲突时无法确定以哪个为基准

### 全局状态是唯一可靠的数据源

