[toc]
# React-redux

## 使用
- 获取store
  - useStore  (hooks)
  - ReactReduxContext.Consumer
    ```
      import { ReactReduxContext } from 'react-redux';
      <ReactReduxContext.Consumer>
        {({ store, subscription }) => {
          // store仅仅是createStore创建的store
          // 通过store.getState()才能获取state
        }}
      </ReactReduxContext.Consumer>
    ```
  - connect()
    - 通过mapStateToProps把state映射到props，此时获取的是state而不是store
- 自定义context
  - 不仅要在Provider设置context属性
  - 而且要在connect时传入context，两种方式选择其一：
    - connect的第四个参数传入context
      ```
        connect(
          mapState,
          mapDispatch,
          null,
          { context: MyContext }
        )(MyComponent)
      ```
    - 调用connected后的组件时传入context作为props
      ```
      //
      const ConnectedComponent = connect(
        mapState,
        mapDispatch
      )(MyComponent)
      // 
      <ConnectedComponent context={MyContext} />
      ```
- 多个自定义context
  - Provider可以嵌套
    ```
    <Provider store={storeA} context={ContextA} />
      <Provider store={storeB} context={ContextB}>
        <RootModule />
      </Provider>
    </Provider>
    ```
  - connect可以组合compose
    ```
    compose(
      connect(mapStateA, null, null, { context: ContextA }),
      connect(mapStateB, null, null, { context: ContextB })
    )(MyComponent);
    ```
### API

- connect()

	- arguments

		- mapStateToProps?: (state, ownProps?) => Object
		- mapDispatchToProps?: Object | (dispatch, ownProps?) => Object
    		- 若此函数为null，则默认为{dispatch}
  			- 类似于redux的bindActionCreators,内部相当于调用`bindActionCreators(mapDispatchToProps, dispatch)`
		- mergeProps?: (stateProps, dispatchProps, ownProps) => Object
    		- 定义如何把stateProps, dispatchProps, ownProps映射到props，默认方式为`{...ownProps, ...stateProps, ...dispatchProps}`
		- options?: Object
        ```
          {
            context?: Object,
            pure?: boolean,
            areStatesEqual?: Function,
            areOwnPropsEqual?: Function,
            areStatePropsEqual?: Function,
            areMergedPropsEqual?: Function,
            forwardRef?: boolean,
          }
        ```
  - returns
    - 返回一个HOC，参数接收组件，返回另一个新组件
- Provider

	- React Component
	- props

		- store 
		- children
		- context

- connectAdvanced()

	- connectAdvanced(selectorFactory, connectOptions?)
  	- selectorFactory可以简单理解为mapDispatchToProps,且其返回值为mapStateToProps
    - 举例
      ```
        function selectorFactory(dispatch) {
          let ownProps = {}
          let result = {}

          const actions = bindActionCreators(actionCreators, dispatch)
          const addTodo = text => actions.addTodo(ownProps.userId, text)

          return (nextState, nextOwnProps) => {
            const todos = nextState.todos[nextOwnProps.userId]
            const nextResult = { ...nextOwnProps, todos, addTodo }
            ownProps = nextOwnProps
            if (!shallowEqual(result, nextResult)) result = nextResult
            return result
          }
        }
      ```
  - batch()
    - 为了确保多个dispatch只产生一次update render
    - 举例
      ```
        import { batch } from 'react-redux'

        function myThunk() {
          return (dispatch, getState) => {
            // should only result in one combined re-render, not two
            batch(() => {
              dispatch(increment())
              dispatch(increment())
            })
          }
        }
      ```
- Hooks

	- hooks

		- useSelector()
		- useDispatch()
		- useStore()

	- custom context

		- useStore = createStoreHook(MyContext)
		- useDispatch = createDispatchHook(MyContext)
		- useSelector = createSelectorHook(MyContext)
		- 举例
      ```
        import React from 'react'
        import {
          Provider,
          createStoreHook,
          createDispatchHook,
          createSelectorHook
        } from 'react-redux'

        const MyContext = React.createContext(null)

        // Export your custom hooks if you wish to use them in other files.
        export const useStore = createStoreHook(MyContext)
        export const useDispatch = createDispatchHook(MyContext)
        export const useSelector = createSelectorHook(MyContext)

        const myStore = createStore(rootReducer)

        export function MyProvider({ children }) {
          return (
            <Provider context={MyContext} store={myStore}>
              {children}
            </Provider>
          )
        }
      ```
	- Hooks Recipes

		- 举例
      ```
        import { bindActionCreators } from 'redux'
        import { useDispatch } from 'react-redux'
        import { useMemo } from 'react'

        export function useActions(actions, deps) {
          const dispatch = useDispatch()
          return useMemo(
            () => {
              if (Array.isArray(actions)) {
                return actions.map(a => bindActionCreators(a, dispatch))
              }
              return bindActionCreators(actions, dispatch)
            },
            deps ? [dispatch, ...deps] : [dispatch]
          )
        }
              - import { useSelector, shallowEqual } from 'react-redux'

        export function useShallowEqualSelector(selector) {
          return useSelector(selector, shallowEqual)
        }
      ```
## 原理

