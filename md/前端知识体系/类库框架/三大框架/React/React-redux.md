# React-redux

## 使用

### API

- connect()

	- arguments

		- mapStateToProps?: (state, ownProps?) => Object
		- mapDispatchToProps?: Object | (dispatch, ownProps?) => Object

			- 类似于redux的bindActionCreators

		- mergeProps?: (stateProps, dispatchProps, ownProps) => Object
		- options?: Object

			- {
  context?: Object,
  pure?: boolean,
  areStatesEqual?: Function,
  areOwnPropsEqual?: Function,
  areStatePropsEqual?: Function,
  areMergedPropsEqual?: Function,
  forwardRef?: boolean,
}

- Provider

	- React Component
	- props

		- store 
		- children
		- context

- connectAdvanced()

	- connectAdvanced(selectorFactory, connectOptions?)

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

			- import React from 'react'
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

	- Hooks Recipes

		- 举例

			- import { bindActionCreators } from 'redux'
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

## 原理

