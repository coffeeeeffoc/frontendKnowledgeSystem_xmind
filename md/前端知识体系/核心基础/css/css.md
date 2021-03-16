# css

## 特性

### 媒体查询

### 特性查询

### 块状格式化上下文BFC

### 外边距重叠
（Collapsing margins）

## 属性

### clear

- 指定一个元素是否必须移动(清除浮动后)到在它之前的浮动元素下面
- 适用于

	- 非浮动元素

		- 将非浮动块的边框边界移动到所有相关浮动元素外边界的下方
		- 非浮动块的垂直外边距会折叠

	- 浮动元素

		- 将元素的外边界移动到所有相关的浮动元素外边框边界的下方
		- 后面的浮动元素的位置无法高于它之前的元素
		- 两个浮动元素的垂直外边距将不会折叠

- 范围

	- 相同BFC范围内

### float

- 如果一个元素里只有浮动元素，那它的高度会是0

## 适配

### 不同浏览器的适配

- 兼容思想

	- 优雅降级

		- 以高版本为准

	- 渐进增强

		- 以低版本为准

- 方案

	- 浏览器默认样式一致

		- reset css

			- 基本所有样式的清零

		- normalize.css

			- 针对跨浏览器的差异进行抹平
			- 优点

				- 保护有用的浏览器默认样式
				- 修复浏览器自身的bug
				- 优化CSS可用性

	- CSS hack
（主要针对IE或某个版本）

		- 条件hack
		- 属性级hack
		- 选择符级hack

	- 自动化插件

		- 浏览器前缀
（Autoprefixer）

### 分辨率适配

## 常见问题

### float导致的高度塌陷

- 方案

	- 为container创建BFC
	- clearfix

		- #container::after {
  content: "";
  display: block;
  clear: both;
}

## 扩展css

### 预编译

- sass

	- sass
	- scss

- less
- Stylus

### 后处理

- postcss

	- 概括

		- 一个使用JS插件来转换样式的工具

	- 架构

		- css ->(解析)-> AST ->(插件转换)-> AST ->(序列化)-> css

	- 特点

		- lint css
		- 支持CSS Next语法
		- css模块化
		- 自动添加前缀(autoprefixer)

	- 插件

		- 使用

			- stylelint

				- 校验

			- stylefmt

				- 自动格式化css

					- 根据stylelint配置文件
					- 配置文件优先级

						- package.json中的stylelint属性
						- .stylelintrc

							- 可以是json或yaml格式
							- 格式

								- .stylelintrc
								- .stylelintrc.json
								- .stylelintrc.yaml
								- .stylelintrc.yml
								- .stylelintrc.js

						- stylelint.config.js
						- stylelint.config.cjs

			- autoprefixer

				- 自动添加浏览器特有前缀

			- postcss-preset-env

				- 可以使用最新css语法

		- 编写插件

	- 与预处理结合使用

		- loader的顺序

			- style-loader
			- css-loader
			- postcss-loader
			- less-loader

		- 写的顺序是自上而下，实际执行时，下面先执行完，再依次上去

## 布局

### 正常布局流

### display

### 弹性盒

### 网格

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

### 多列布局

- column-count

	- 设置列的个数

- column-width

	- 设置列的宽度

### css表格布局

- display: table

### 响应式布局

