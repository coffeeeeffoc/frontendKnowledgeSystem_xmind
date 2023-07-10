# 块格式化上下文BFC
块格式化上下文对浮动定位（参见 float）与清除浮动（参见 clear）都很重要。浮动定位和清除浮动时只会应用于同一个BFC内的元素。浮动不会影响其它BFC中元素的布局，而清除浮动只能清除同一BFC中在它前面的元素的浮动。外边距折叠（Margin collapsing）也只会发生在属于同一BFC的块级元素之间。
## 创建BFC的方式

### 根元素（<html>）

### 浮动元素（元素的 float 不是 none）

### overflow的不为visible的块元素

### 绝对定位元素（元素的 position 为 absolute 或 fixed）

### 行内块元素（元素的 display 为 inline-block）

### 表格单元格（元素的 display 为 table-cell，HTML表格单元格默认为该值）

### display为table、table-*

### display: flow-root

### 弹性元素（display 为 flex 或 inline-flex 元素的直接子元素）

### 网格元素（display 为 grid 或 inline-grid 元素的直接子元素）

### ...

## 范围

### 块格式化上下文包含创建它的元素内部的所有内容

## 特点

### 浮动定位和清除浮动时只会应用于同一个BFC内的元素

### 浮动不会影响其它BFC中元素的布局，而清除浮动只能清除同一BFC中在它前面的元素的浮动

## 场景

### 让浮动内容和周围的内容等高
(在浮动元素的父容器上设置)

- 现象

	- 容器内某元素为浮动，浮动元素的高度与父容器的高度不一致

- 设置位置

	- 父容器上设置

- 解决办法

	- 创建一个会包含这个浮动的 BFC

		- <style>
  section {
    height:150px;
}
.box {
    background-color: rgb(224, 206, 247);
    border: 5px solid rebeccapurple;
}
.box[style] {
    background-color: aliceblue;
    border: 5px solid steelblue;
}
.float {
    float: left;
    overflow: hidden; /* required by resize:both */
    resize: both;
    margin-right:25px;
    width: 200px;
    height: 100px;
    background-color: rgba(255, 255, 255, .75);
    border: 1px solid black;
    padding: 10px;
}
</style>
<section>
  <div class="float">Try to resize this outer float</div>
  <div class="box"><p>Normal</p></div>
</section>
<section>
  <div class="float">Try to resize this outer float</div>
  <div class="box" style="display:flow-root"><p><code>display:flow-root</code><p></div>
</section>


### （父、子元素）外边距重叠

- 现象

	- 父元素与子元素margin重叠

- 设置位置

	- 在父元素上设置

- 解决办法

	- 父元素创建BFC

		- <style>
.blue, .red-inner {
  height: 50px;
  margin: 10px 0;
}

.blue {
  background: blue;
}

.red-outer {
  overflow: hidden;
  background: red;
}
</style>
<div class="blue"></div>
<div class="red-outer">
  <div class="red-inner">red inner</div>
</div>

### 排除外部浮动

- 现象

	- 浮动的元素影响兄弟元素的布局

- 设置位置

	- 浮动的兄弟元素上设置

- 解决办法

	- 兄弟元素创建BFC

