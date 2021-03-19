[toc]
# 作用
访问dom节点或子React组件
# 使用场景
- 管理焦点
- 调用子组件的方法
- 集成第三方DOM库

# 设置ref
- class
  - React.createRef(null);
  - `ref={ref => variable = ref}`
  - ref
- function
  - useRef(null);
  - useImperativeHandle(ref, () => ({...rest}))
    - 可以指定父组件访问ref.current的属性(即rest)
    - 示例：
    ```
  function Child(props, parentRef) {
    let inputRef = useRef();
    useImperativeHandle(parentRef, () => ({
      focus() {inputRef.current.focus();},
      setValue(newVal) {inputRef.current.value = newVal;}
    }));
    return <>...</>
  }
  const ForwardedChild = forwardRef(Child);
  function Parent() {
    let [number, setNumber] = useState(0);
    let parentRef = useRef();
    return <ForwardedChild ref={parentRef} />
  }
    ``` 
# 传递ref
- var Comp = React.forwardRef((props, ref) => <Com />)
  - 注意，第二个参数 ref 只在使用 React.forwardRef 定义组件时存在。常规函数和 class 组件不接收 ref 参数，且 props 中也不存在 ref
- 采用自定义属性（非key、非ref即可），从props传递给子组件
# 使用ref