# 作用
允许异步的dispatch某个action
# 使用
- 基本使用
  ```
  const fn = (dispatch, getState) => {
    // ...在合适的时机，调用dispatch(args),其中args为类似于fn的函数或者action对象`{type:'xxx',...payload}`
    //若返回promise，则dispatch之后可以调用.then()方法
  }
  dispatch(fn);
  若fn返回promise，则可以使用dispatch(fn).then(() => {});
  ```
- 注入自定义参数
  ```
  const store = createStore(
    reducer,
    applyMiddleware(thunk.withExtraArgument(api)),
  );

  // later
  function fetchUser(id) {
    return (dispatch, getState, api) => {
      // you can use api here
    };
  }
  ```
# 源码
这就是全部源码：
```
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => (next) => (action) => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```