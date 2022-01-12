[toc]

# React

## 使用

### 组件通信

- props

	- 父到子
	- callback子到父

- Context

	- 子主题 1

- Redux

### 懒加载

### API

- React

	- 组件

		- Component
		- PureComponent
		- memo(Comp)

	- 创建元素

		- createElement()
		- createFactory()

	- 转换元素

		- cloneElement()
		- isValidElement()
		- React.Children

	- Fragments

		- Fragment

	- Refs

		- React.createRef
		- React.forwardRef

	- Suspense

		- lazy
		- Suspense

	- Context

		- createContext(defaultValue)

	- Hook

		- 基础

			- useState
			- useEffect
			- useContext

		- 额外

			- useReducer
			- useCallback
			- useMemo
			- useRef
			- useImperativeHandle
			- useLayoutEffect
			- useDebugValue

- React.Component

	- 生命周期

		- 挂载

			- constructor(props)
			- static getDerivedStateFromProps(props, state)
			- render()
			- componentDidMount()

		- 更新

			- static getDerivedStateFromProps(props, state)
			- shouldComponentUpdate(nextProps, nextState)
			- render()
			- getSnapshotBeforeUpdate(prevProps, prevState)
			- componentDidUpdate(prevProps, prevState, snapshot)

		- 卸载

			- componentWillUnmount()

		- 错误处理

			- static getDerivedStateFromError(error)
			- componentDidCatch(error, info)

	- 其他API

		- setState
		- forceUpdate

	- class属性

		- defaultProps
		- displayName

	- 实例属性

		- props
		- state

- ReactDom

	- render(element, container[, callback])
	- hydrate(element, container[, callback])

		- 与 render() 相同，但它用于在 ReactDOMServer 渲染的容器中对 HTML 的内容进行 hydrate 操作

	- unmountComponentAtNode(container)
	- findDOMNode(component)

		- 不能用于函数组件
		- 只在已挂载的组件上可用

	- createPortal(child, container)

- ReactDOMServer

	- renderToString(element)
	- renderToStaticMarkup(element)
	- renderToNodeStream(element)

		- 只能在服务端使用

	- renderToStaticNodeStream(element)

		- 只能在服务端使用

- Dom元素

	- 属性差异

		- checked

			- input的type为checkbox或radio，checked对应受控组件属性，defaultChecked对应非受控组件属性

		- className

			- Web Components中必须用class而不能用className
			- v16可以支持class，但是仍然推荐className

		- dangerouslySetInnerHTML

			- <div dangerouslySetInnerHTML={{__html:'eee'}} />

		- htmlFor

			- 比如label标签的for

		- onChange

			- 表单变化时即触发，而不是如浏览器默认行为（行为和名称不对应）

		- selected

			- 原生选中，需要在option标签添加selected属性
			- React中，需要在select标签中使用value属性控制，而不是在option中使用selected控制

		- style

			- 小驼峰作为key的对象，而不是字符串
			- 预防跨站脚本（XSS）
			- 单位为px的数字，后面会自动添加px

		- suppressContentEditableWarning

			- 当拥有子节点的元素被标记为 contentEditable 时，React 会发出一个警告，因为这不会生效。该属性将禁止此警告

		- value

			- `<input>、<select> 和 <textarea>` 组件支持 value 属性；受控组件使用value，非受控组件使用defaultValue

- 合成事件

	- v16绑定在document上，v17绑定在rootNode上
	- 处理函数在冒泡阶段被触发
	- 如需注册捕获阶段的事件处理函数，则应为事件名添加 Capture。例如，处理捕获阶段的点击事件请使用 onClickCapture，而不是 onClick
	- 合成做了些啥

		- 对原生事件的封装

			- 原生事件对象被放在了属性 e.nativeEvent内

		- 对某些原生事件的升级和改造
		- 不同浏览器事件兼容的处理

	- 合成的作用

		- 减少事件注册，减少内存消耗，提升性能
		- 统一规范，解决 ie 事件兼容问题，简化事件逻辑
		- 对开发者友好，调用更符合语义

	- 不与原生事件混用

		- 原生事件（阻止冒泡）会阻止合成事件的执行
		- 合成事件（阻止冒泡）不会阻止原生事件的执行
		- 两者最好不要混合使用，避免出现一些奇怪的问题

## 关联库

### 路由

- react-router-dom

### 状态管理

- redux
- react-redux
- dva

	- redux-saga

## [生命周期](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

### v16.3

- 

### v16.4+

- 

### 三个阶段

- render
- pre-commit
- commit

## 版本

## hooks

## 架构

### v15

- Reconciler协调器

	- 负责找出变化的组件

- Renderer 渲染器

	- 负责将变化的组件渲染到页面上

### v16

### v17

## 最佳实践

### 组合 vs 继承

- 推荐组合，而不是继承
- 组合

	- 把组件A实例a作为props或者children传递给组件B，在B内合适的位置渲染a的内容

## React源码分析

## 数据流方案

### Flux

- 单向数据流
- 代表

	- redux

### Reactive

- 响应式数据流方案
- 代表

	- Mobx

### rxjs

