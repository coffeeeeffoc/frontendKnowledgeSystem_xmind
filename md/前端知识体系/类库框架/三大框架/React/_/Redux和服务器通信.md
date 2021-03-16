# Redux和服务器通信

## react组件访问服务器

### 代理功能

- create-react-app有代理功能，只需要在package.json中添加一行：
"proxy": "http://www.weather.com.cn"

### 在组件的componentDidMount函数中请求服务器

- 装载完成，可能请求依赖于部分已经渲染的数据，所以要在此处执行

### 使用fetch函数获取

- response.status === 200时才算成功
- response.json()返回一个promise对象，调用then方法，参数就是真正的回复数据

### React组件访问服务器

- 优点

	- 操作简单、容易理解

- 缺点

	- 组件复杂时，状态放在组件内不合适，应该放在redux

## redux访问服务器

