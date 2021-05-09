[toc]

# [MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)

## 构造函数

## 方法
### disconnect()
### observe(): (target: Node | Element, ?option: MutationObserverInit) => void
- 语法：`mutationObserver.observe(target[, options])`
- MutationObserverInit
  - 属性,childList，attributes 或者 characterData 三个属性之中，至少有一个必须为 true，否则会抛出 TypeError 异常
    - ?attributeFilter: string[],
    - ?attributeOldValue: boolean
    - ?attributes: boolean
    - ?characterData: boolean
    - ?characterDataOldValue: boolean
    - ?childList: boolean
    - ?subtree: boolean
### takeRecords(): void => MutationRecord
返回已检测到但尚未由观察者的回调函数处理的所有匹配DOM更改的列表，使变更队列保持为空。 此方法最常见的使用场景是在断开观察者之前立即获取所有未处理的更改记录，以便在停止观察者时可以处理任何未处理的更改。
可与构造函数共用同一个callback
```js
const mutations = observer.takeRecords();
if (mutations) {
  callback(mutations);
}
observer.disconnect();
```
## 例子
```js
const config = { attributes: true, childList: true, subtree: true };
const callback = (mutationsList, observer) => {
 for(let mutation of mutationsList) {
 }
};
const observer = new MutationObserver(callback);
observer.observe(targetNode, config);
observer.disconnect();
```