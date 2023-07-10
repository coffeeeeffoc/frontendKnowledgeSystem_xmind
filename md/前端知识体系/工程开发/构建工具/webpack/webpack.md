# webpack

## 原理

## 使用

## 临时抱佛脚

### 使用

- 配置文件

	- 使用配置文件

		- npx webpack [--config webpack.config.js]

	- 不使用配置文件

		- npx webpack --entry <entry> -o <output-path>

	- 默认配置

		- 优先级

			-  .webpack/webpackfile > .webpack/webpack.config.js > webpack.config.js

	- 指定部分配置

		- npx webpack --config-name first --config-name second

			- 指定导出的配置数组中name为first和second的配置项，忽略其他配置项

### loader

- webpack 只能理解 JavaScript 和 JSON 文件

### loader和插件的作用

- loader

	- 用于转换某些类型的模块

- 插件

	- 用于执行范围更广的任务

		- 打包优化
		- 资源管理
		- 注入环境变量
### 插件
#### UglifyJsPlugin
去掉无效代码、去掉日志输入代码、缩短变量名
#### uglify-webpack-plugin
直接压缩压缩ES6代码

### mode

- development
- production

	- 默认

- none

### 子主题 2

## 源码分析

## 手写

### 手写webpack

### 手写plugin

### 手写loader

### 手写scaffold（脚手架）

