# 实践1
```
<style>
  .wrapper {
    display: flex;
    width: 800px;
    border: 1px solid rgb(190, 122, 185);
  }
  .a1,.a2,.a3,.a4 {
    border: 1px solid;
    margin: 2px;
    width: 100px;
    margin: 0 10px;
    padding: 0 10px;
    margin: 0 10px;
    height: 100px;
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 100px;
  }
  .a4 {
    flex-basis: 0;
  }
</style>
<div class="wrapper">
  <div class="a1"></div>
  <div class="a2"></div>
  <div class="a3"></div>
  <div class="a4"></div>
</div>
```
<code>
  <style>
    .wrapper {
      display: flex;
      width: 800px;
      border: 1px solid rgb(190, 122, 185);
    }
    .a1,.a2,.a3,.a4 {
      border: 1px solid;
      margin: 2px;
      width: 100px;
      margin: 0 10px;
      padding: 0 10px;
      margin: 0 10px;
      height: 100px;
      flex-grow: 1;
      flex-shrink: 1;
      flex-basis: 100px;
    }
    .a4 {
      flex-basis: 0;
    }
  </style>
  <div class="wrapper">
    <div class="a1"></div>
    <div class="a2"></div>
    <div class="a3"></div>
    <div class="a4"></div>
  </div>
</code>
## 小结
- 容器元素和子项目元素都不设置box-sizing情况下
  - flex子项目元素平的容器元素的content-box
  - 计算剩余空间时，减去子项目元素的margin、border、padding
  - 剩余空间分配给子项目元素的content-box
# 实践2
```
  <style>
    .wrapper {
      display: flex;
      width: 800px;
      border: 1px solid rgb(190, 122, 185);
    }
    .a1,.a2,.a3,.a4 {
      border: 1px solid;
      margin: 2px;
      width: 100px;
      margin: 0 10px;
      padding: 0 10px;
      margin: 0 10px;
      height: 100px;
      flex-grow: 1;
      flex-shrink: 1;
      flex-basis: 100px;
    }
    .a4 {
      flex-basis: 0;
      box-sizing: border-box; /* + */
    }
  </style>
  <div class="wrapper">
    <div class="a1"></div>
    <div class="a2"></div>
    <div class="a3"></div>
    <div class="a4"></div>
  </div>
```
<code>
  <style>
    .wrapper {
      display: flex;
      width: 800px;
      border: 1px solid rgb(190, 122, 185);
    }
    .a1,.a2,.a3,.a4 {
      border: 1px solid;
      margin: 2px;
      width: 100px;
      margin: 0 10px;
      padding: 0 10px;
      margin: 0 10px;
      height: 100px;
      flex-grow: 1;
      flex-shrink: 1;
      flex-basis: 100px;
    }
    .a4 {
      flex-basis: 0;
      box-sizing: border-box; /* + */
    }
  </style>
  <div class="wrapper">
    <div class="a1"></div>
    <div class="a2"></div>
    <div class="a3"></div>
    <div class="a4"></div>
  </div>
</code>
## 现象
 - 子项目元素设置box-sizing: border-box;时所有元素的宽度并没有发生变化

## 小结
 - 子项目元素设置box-sizing: border-box;时
   - 剩余空间分配给子项目元素的content-box;
# 实践3
```
  <style>
    .wrapper {
      display: flex;
      width: 800px;
      border: 1px solid rgb(190, 122, 185);
      box-sizing: border-box; /* + */
    }
    .a1,.a2,.a3,.a4 {
      border: 1px solid;
      margin: 2px;
      width: 100px;
      margin: 0 10px;
      padding: 0 10px;
      margin: 0 10px;
      height: 100px;
      flex-grow: 1;
      flex-shrink: 1;
      flex-basis: 100px;
    }
    .a4 {
      flex-basis: 0;
      box-sizing: border-box; /* + */
    }
  </style>
  <div class="wrapper">
    <div class="a1"></div>
    <div class="a2"></div>
    <div class="a3"></div>
    <div class="a4"></div>
  </div>
```
<code>
  <style>
    .wrapper {
      display: flex;
      width: 800px;
      border: 1px solid rgb(190, 122, 185);
      box-sizing: border-box; /* + */
    }
    .a1,.a2,.a3,.a4 {
      border: 1px solid;
      margin: 2px;
      width: 100px;
      margin: 0 10px;
      padding: 0 10px;
      margin: 0 10px;
      height: 100px;
      flex-grow: 1;
      flex-shrink: 1;
      flex-basis: 100px;
    }
    .a4 {
      flex-basis: 0;
      box-sizing: border-box; /* + */
    }
  </style>
  <div class="wrapper">
    <div class="a1"></div>
    <div class="a2"></div>
    <div class="a3"></div>
    <div class="a4"></div>
  </div>
</code>
## 现象
- 父元素的content-width变成798
- 第4个子项目元素width从83变成82.5
## 小结
- 父元素的剩余空间是content-box计算的


# 结论
-  不论box-sizing，剩余的空间分配时，都是分配给子项目元素的content-box;
- 不论box-sizing，计算剩余的空间时，都是从容器元素的content-box中减去flex-basis的基准空间;