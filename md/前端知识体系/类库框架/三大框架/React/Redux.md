# Redux

## API

### createStore(reducer, [preloadedState], [enhancer])

- arguments

	- reducer (Function):

		- type Reducer<S, A> = (state: S, action: A) => S

	- [preloadedState] (any)

		- The initial state

	- [enhancer] (Function)

		- 只能是applyMiddleware(xxx)，其中xxx是中间件

- returns

	- Store，包含全部状态的对象

### Store

- Methods

	- getState()
	- dispatch(action)

		- action为一个对象，至少有一个type字段，其他的字段根据需要而定
		- {type:'xx',...rest}

	- subscribe(listener)

		- arguments

			- listener(Function)，无参数

		- returns

			- (Function)，unsubscribe

	- replaceReducer(nextReducer)

		- arguments

			- nextReducer (Function)

### combineReducers(reducers)

- arguments

	- reducers (Object)

		- {key1: reducer1,key2: reducer2}
		- 其中reducer最终生成的state就是包含key1和key2的对象，比如{key1:obj1,key2:obj2}

- returns

	- (Function)

		- 此reducer函数，会按照key1和key2依次映射调用各自的reducer，然后生成最终的state对象

- 注意，每个reducer需满足条件

	- 如果action无法识别，则原封不动返回state
	- 不能返回undefined
	- 需要给reducer的第一个参数state设置默认值

### applyMiddleware(...middleware)

- arguments

	- ...middleware (arguments)

		- 每个middleware的格式为

			- ({ getState, dispatch }) => next => action

- returns

	- (Function)

		- A store enhancer 

			- enhancer的格式

				- createStore => createStore

- 使用方式

	- const store = createStore(todos, ['Use Redux'], applyMiddleware(logger))

### bindActionCreators(actionCreators, dispatch)

- arguments

	- actionCreators (Function or Object)

		- An action creator, or an object whose values are action creators

	- dispatch (Function)

- returns
- 举例

	- function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}
function removeTodo(id) {
  return {
    type: 'REMOVE_TODO',
    id
  }
}
const obj1 = {
  addTodo,
  removeTodo,
};
const obj2 = bindActionCreators(obj1, dispatch);
obj2.addTodo() 等价于 dispatch(obj1.addTodo);

### compose(...functions)

- arguments

	- (arguments)

- returns

	- (Function)

		- The final function obtained by composing the given functions from right to left

## 三大原则

### 单一数据源

### 状态只读

### 纯函数执行修改

