# 浏览器js加载

## 异步加载方式 

### async

- 下载完就执行。中断当前渲染
- 不保证执行顺序

### defer

- 渲染完执行。渲染完毕且其他js执行完毕才执行
- 保证执行顺序与加载顺序一致

### async vs defer
https://stackoverflow.com/a/39711009

## ES6模块加载

### type="module"属性

### 默认采用defer异步加载方式

- 等待渲染完毕、其他脚本执行完毕，才执行本脚本
- 多个<script type="module">按顺序依次执行

### 如果设置async属性，则，采用async方式加载

