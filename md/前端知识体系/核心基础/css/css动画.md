# css动画

## animation

### 属性

- animation-name
- animation-delay
- animation-direction

	- 值

		- normal | reverse | alternate | alternate-reverse

- animation-duration

	- ms或者s

- animation-fill-mode

	- 值

		- none 不改变默认行为。
		- forwards 当动画完成后，保持最后一个属性值（在最后一个关键帧中定义）。
		- backwards 在 animation-delay 所指定的一段时间内，在动画显示之前，应用开始属性值（在第一个关键帧中定义）。
		- both 向前和向后填充模式都被应用。

	- 一个解释，简明易懂
https://segmentfault.com/q/1010000003867335

- animation-iteration-count

	- 循环次数
	- 值

		- infinite
		- 数值<number>

- animation-play-state

	- 值

		- running
		- paused

- animation-timing-function

	- 可能值为一或多个 <timing-function>
	-  作用于一个关键帧周期而非整个动画周期，即从关键帧开始开始，到关键帧结束结束

### 属性之间没发现什么顺序的要求，duration先于delay

### 注意点

- 创建动画的原理是，将一套 CSS 样式逐渐变化为另一套样式。
- 通过 @keyframes 规则，您能够创建动画。
- @keyframes定义一个动画，并定义具体的动画效果，比如是放大还是位移等等。
- @keyframes 它定义的动画并不直接执行，需要借助animation来运转。
- 在动画过程中，您能够多次改变这套 CSS 样式。
- 以百分比来规定改变发生的时间，或者通过关键词 "from" 和 "to"，等价于 0% 和 100%。

	- 百分比是指动画完成一遍的时间长度的的百分比 ，0% 是动画的开始时间，50%是动画完成一半的时间，100% 动画的结束时间。百分比后面的花括号写：在动画执行过程中的某时间点要完成的变化。

- 为了获得最佳的浏览器支持，您应该始终定义 0% 和 100% 选择器。

## transition

### 属性

- transition-property
- transition-duration
- transition-timing-function
- transition-delay

### 允许css的属性值在一定的时间区间内平滑地过渡

## transform

### 旋转rotate（中心为原点）

### 扭曲、倾斜skew（skew(x,y), skewX(x), skewY(y)）

### 缩放scale（scale(x,y), scaleX(x), scaleY(y)）

### 移动translate（translate(x,y),translateX(x),translateY(y)）

### 矩阵变形matrix。

