# React源码分析

## 事件

### [合成事件机制原理](https://segmentfault.com/a/1190000039108951)

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

