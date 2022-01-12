# React源码分析

## 事件

### [合成事件机制原理](https://segmentfault.com/a/1190000039108951)

## 批量更新
- v17及以下
	在合成事件中（比如onClick事件），多次的setState会合并，只触发一次render。
	Promise、setTimeout、原生事件、以及其他事件中，则每次setState都会同步触发一次render。
- v18
  - [Automatic batching](https://github.com/reactwg/react-18/discussions/21)
  - 不想自动批量更新怎么办？
  - ReactDOM.flushSync(() => {});
- ReactDOM.unstable_batchedUpdates
  - 在v18中继续生效可以使用，不过由于自动批量更新，所以没必要使用。
## [startTransition](https://github.com/reactwg/react-18/discussions/41)
- 目的
  - 解决立即响应和延迟响应的矛盾，优雅解决延迟响应
    - 与setTimeout、throttle、debounce对比
      - 前者能更早的响应
        - 因为前者根据实际情况调度，而后三者都是指定时间，且指定的时间不太好控制
      - 前者不会后续卡顿；后三者，当延迟到真正执行的时候，依然有卡顿的问题
      - 前者在react内部处理，后者需开发者自行编码处理
- 使用场景
  - 减少rerender依然是主流优化方案，startTransition只是补充
  - 延迟响应的场合，比如缓慢渲染，网络请求等
## [优先级](https://segmentfault.com/a/1190000038947307)

### 事件优先级

- 按照用户事件的交互紧急程度，划分的优先级
- 按紧急程度，三个优先级

	- 离散事件（DiscreteEvent）

		- click、keydown、focusin等，这些事件的触发不是连续的，优先级为0

	- 用户阻塞事件（UserBlockingEvent）

		- drag、scroll、mouseover等，特点是连续触发，阻塞渲染，优先级为1

	- 连续事件（ContinuousEvent）

		- canplay、error、audio标签的timeupdate和canplay，优先级最高，为2

### 更新优先级

- 事件导致React产生的更新对象（update）的优先级（update.lane）

### 任务优先级

- 产生更新对象之后，React去执行一个更新任务，这个任务所持有的优先级

### 调度优先级

- Scheduler依据React更新任务生成一个调度任务，这个调度任务所持有的优先级

## 优先级（需要自己调试源码）

### eventPriority 

### lanePriority

### SchedulerPriority

