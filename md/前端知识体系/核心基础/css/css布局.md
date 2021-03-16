[toc]
# css布局

## 种类

### 正常布局流

### display

### 弹性盒

- 影响

	- 子元素的float、clear和vertical-align属性将失效

- 属性

	- 容器属性

		- flex-direction

			- row | row-reverse | column | column-reverse;

				- row（默认值）：主轴为水平方向，起点在左端
				- row-reverse：主轴为水平方向，起点在右端
				- column：主轴为垂直方向，起点在上沿
				- column-reverse：主轴为垂直方向，起点在下沿

			- 主轴的方向（即项目的排列方向）
			- 

		- flex-wrap

			- nowrap | wrap | wrap-reverse;

				- wrap-reverse

					- 

				- nowrap（默认）：不换行

					- 

				- wrap：换行，第一行在上方

					- 

			- 一条轴线排不下，如何换行
			- 

		- flex-flow

			- `<flex-direction> || <flex-wrap>`

				- 默认值为row nowrap

			- flex-direction属性和flex-wrap属性的简写形式

		- justify-content

			- 项目在主轴上的对齐方式
			- flex-start | flex-end | center | space-between | space-around;

				- flex-start（默认值）：左对齐
				- flex-end：右对齐
				- center： 居中
				- space-between：两端对齐，项目之间的间隔都相等。
				- space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

			- 

		- align-items

			- flex-start | flex-end | center | baseline | stretch;

				- flex-start：交叉轴的起点对齐。
				- flex-end：交叉轴的终点对齐。
				- center：交叉轴的中点对齐。
				- baseline: 项目的第一行文字的基线对齐。
				- stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

			- 交叉轴上如何对齐
			- 

		- align-content

			- flex-start | flex-end | center | space-between | space-around | stretch;

				- flex-start：与交叉轴的起点对齐。
				- flex-end：与交叉轴的终点对齐。
				- center：与交叉轴的中点对齐。
				- space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
				- space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
				- stretch（默认值）：轴线占满整个交叉轴

			- 多根轴线的对齐方式
			- 

	- 项目属性

		- order

			- `<integer>;`
			- 项目的排列顺序。数值越小，排列越靠前，默认为0

		- flex-grow

			- `<number>;` /* default 0 */
			- 项目的放大比例，默认为0，即如果存在剩余空间，也不放大

				- 注：放大的比例是content-width的放大比例，而不是box-sizing对应的width的放大比例。

			- 如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）

		- flex-shrink

			- `<number>`; /* default 1 */
			- 项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
			- 如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小
			- 如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小

		- flex-basis

			- `<length>` | auto; /* default auto */
			- 分配多余空间之前，项目占据的主轴空间（main size）

				- 注：所占的空间指的width由box-sizing决定。

			- 它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间
			- 取值范围
    			- `<length> | auto`;
        			-  默认为auto，auto表示宽、高取决于width和height属性的值
    			- content;表示宽、高取决于内容自身的宽高，而不是width、height设置的宽高。

		- flex

			- none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
			- flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto
			- 两个快捷

				- auto

					- 1 1 auto

				- none

					- 0 0 auto
			- flex:n的含义
    			- 等价于flex-grow:n;flex-shrink:1;flex-basis:0%;
		- align-self

			- auto | flex-start | flex-end | center | baseline | stretch;
			- 允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性
			- 默认值为auto

### 网格

- 基本概念

	- 容器和项目

		- Grid 布局只对项目生效
		- 项目只能是容器的顶层子元素，不包含项目的子元素

	- 行和列

		- 容器里面的水平区域称为"行"（row），垂直区域称为"列"（column）

	- 单元格

		- 行和列的交叉区域，称为"单元格"（cell）
		- n行和m列会产生n x m个单元格

	- 网格线

		- 划分网格的线，称为"网格线"（grid line）
		- n行有n + 1根水平网格线

- 属性

	- 容器属性

		- display

			- grid || inline-grid

		- grid-template-columns
grid-template-rows

			- 定义每一列的列宽/行高

				- 单位可以是px绝对单位，也可以是百分比相对单位

			- 语法

				- 子主题 1

			- 函数

				- repeat()

					- repeat( [ <positive-integer> | auto-fill | auto-fit ] , <track-list> )

						- 第一个参数

							- 重复的次数
							- auto-fill

								- 个数不确定，根据第二个参数自动填充

							- auto-fit

						- 第二个参数<track-list>

							- 宽度

								- 20px

							- 一种模式

								- 20px 30px

						- 例子

							- grid-template-columns: repeat(2, 100px 20px 80px);
							- grid-template-columns: repeat(3, 33.33%)
							- grid-template-columns: repeat(auto-fill, 100px);

				- minmax()

					- 产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值
					- 语法

						- minmax( [ <length> | <percentage> | min-content | max-content | auto ] , [ <length> | <percentage> | <flex> | min-content | max-content | auto ] )

			- 关键字

				- fr

					- 片段，代表一个宽度

				- auto

					- 表示由浏览器自己决定长度
					- grid-template-columns: 100px auto 100px;

				- 网格线的名称

					- grid-template-columns属性和grid-template-rows属性里面，还可以使用方括号，指定每一根网格线的名字，方便以后的引用
					- 网格布局允许同一根线有多个名字，比如[fifth-line row-5]
					- .container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}

		- grid-row-gap
grid-column-gap
grid-gap

			- 行间距
列间距
<grid-row-gap> <grid-column-gap>

		- grid-template-areas

			- 用于定义区域
			- 语法

				- grid-template-areas: "a b b"
                     "a c d";

					- 其中多个a代表组成的矩形，构成a区域
					- 可以在项目属性中设置grid-area: a;来引用这些变量

		- grid-auto-flow 

			- 语法

				- [ row | column ] || dense

					- 含义

						- row

							- 按行优先排列

						- column

							- 按列优先排列

						- dense

							- "稠密”堆积算法，如果后续元素能填充前面的空隙，则填充
							- 会导致顺序错乱

					- 可能

						- row
						- row dense
						- column
						- column dense
						- dense

		- justify-items
align-items
place-items

			- 语法

				- start | end | center | stretch;

					- start：对齐单元格的起始边缘。
end：对齐单元格的结束边缘。
center：单元格内部居中。
stretch：拉伸，占满单元格的整个宽度（默认值）

		- justify-content
align-content
place-content

			- 语法

				- start | end | center | stretch | space-around | space-between | space-evenly;

		- grid-auto-columns
grid-auto-rows

			- 语法与grid-template-columns和grid-template-rows相同
			- 用来设置当template-*之外仍然有多余的元素时，自动创建的网格的属性

		- grid-template

			- grid-template-columns、grid-template-rows和grid-template-areas这三个属性的合并简写形式

		- grid

			- grid-template-rows、grid-template-columns、grid-template-areas、 grid-auto-rows、grid-auto-columns、grid-auto-flow这六个属性的合并简写形式

	- 项目属性

		- grid-column-start
grid-column-end
grid-row-start
grid-row-end

			- 语法

				- auto | <custom-ident> | [ <integer> && <custom-ident>? ] | [ span && [ <integer> || <custom-ident> ] ]

					- integer为数字序号
					- custom-ident为网格线引用

			- 指定项目的四个边框，分别定位在哪根网格线

		- grid-column
grid-row

			- grid-column-start
grid-column-end的简写

grid-row-start
grid-row-end的简写
			- 例子

				- .item-1 {
  grid-column: 1 / 3;
  grid-row: 1 / 2;
}
/* 等同于 */
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 1;
  grid-row-end: 2;
}

		- grid-area

			- 语法

				- <grid-line> [ / <grid-line> ]{0,3}where <grid-line> = auto | <custom-ident> | [ <integer> && <custom-ident>? ] | [ span && [ <integer> || <custom-ident> ] ]

			- 例子

				- grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
				- grid-area: 2 / 2 / auto / span 3;
				- grid-area: some-grid-area;
grid-area: some-grid-area / another-grid-area;

		- justify-self
align-self 
place-self

			- 语法同justify-items、align-items、place-items等属性，仅可针对单个项目

### 浮动

### 定位

- 静态定位static

	- 文档布局流的默认位置

- 相对定位relative

	- 相对于元素在正常的文档流中的位置移动它

- 绝对定位absolute

	- 相对html元素、或者相对最近的被定位的祖先元素

- 固定定位fixed

	- 相对浏览器视口固定

- 粘性定位sticky

	- 相对视口viewport达到预设值时，类似fixed定位；没达到预设值时，为普通流
	- 作为最小的值。比如设置为top:20px时，距离上面>20px时正常流，小于等于20px时，等于20px。
	- 根据正常文档流，相对最近的拥有滚动机制的祖先
    	- 滚动机制，指的是overflow 是 hidden, scroll, auto, 或 overlay的祖先

### 多列布局

- column-count

	- 设置列的个数
	- 语法

		- column-count: 3;
		- column-count: auto;

		- column-count: inherit;
		- column-count: initial;
		- column-count: unset;

    - auto，表示列的数量有column-width决定
    - number，严格正数，最大列数

- column-width

	- 设置列的宽度

### css表格布局

- display: table

### 响应式布局

- 只是一个概念
- 包括

	- 媒体查询
	- 百分比设置
	- 多列布局
	- flex布局
	- grid布局
	- 响应式排版

		- 字体单位

			- em

				- 在 font-size 中使用是相对于父元素的字体大小，在其他属性中使用是相对于自身的字体大小，如 width

			- rem

				- 根元素的字体大小

			- vw

				- 视窗宽度的1%

			- vh

				- 视窗高度的1%

		- 视口元标签

			- <meta name="viewport" content="width=device-width,initial-scale=1">

## 唯二可靠且跨浏览器兼容的布局方式

### float

### position

