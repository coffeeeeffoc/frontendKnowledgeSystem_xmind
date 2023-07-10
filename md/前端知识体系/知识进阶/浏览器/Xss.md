[toc]
# XSS 
 Cross Site Scripting，跨站脚本

## 危害

### 窃取cookie
### 监听用户行为
### 修改dom
### 页面内生成广告

## 注入方式
### 存储型
- 服务器会存储恶意脚本
### 反射型
- 服务器不会存储恶意脚本
### 基于DOM
- 网络劫持
  网络传输过程修改web页面数据
  - WIFI路由器劫持
  - 本地恶意软件劫持

## 阻止XSS攻击
### 服务器对输入的脚本进行过滤或转码
- html标签转义
  如`<script>alert('你被xss攻击了')</script>`被转义为`&lt;script&gt;alert(&#39;你被xss攻击了&#39;)&lt;/script&gt;`
- 禁止以 javascript: 开头的链接，和其他非法的 scheme
- HTML 转义是非常复杂的，在不同的情况下要采用不同的转义规则，应当采用成熟的、业界通用的转义库
### 充分利用CSP
#### 配置方式
- meta标签
  ```html
  <meta http-equiv="Content-Security-Policy"
      content="default-src 'self'; img-src https://*; child-src 'none';">
  ```
- http header
  ```
  Content-Security-Policy: default-src 'self'
  Content-Security-Policy: default-src 'self' trusted.com *.trusted.com
  Content-Security-Policy: default-src 'self'; img-src *; media-src media1.com media2.com; script-src userscripts.example.com
  Content-Security-Policy: default-src https://onlinebanking.jumbobank.com
  Content-Security-Policy: default-src 'self' *.mailsite.com; img-src *

  Content-Security-Policy: default-src 'self'; report-uri http://reportcollector.example.com/collector.cgi
  Content-Security-Policy: default-src 'none'; style-src cdn.example.com; report-uri /_/csp-reports

  ```
#### 限制内容
- 限制加载其他域下的资源文件，这样即使黑客插入了一个 JavaScript 文件，这个 JavaScript 文件也是无法被加载的；
- 禁止向第三方域提交数据，这样用户数据也不会外泄；
- 禁止执行内联脚本和未授权的脚本；
- 还提供了上报机制，这样可以帮助我们尽快发现有哪些 XSS 攻击，以便尽快修复问题。
### 使用HttpOnly属性
无法通过document.cookie获取HttpOnly的cookie
## React中的XSS
### 自动转义
React 在渲染 HTML 内容和渲染 DOM 属性时都会将 "'&<> 这几个字符进行转义，转义部分源码如下：
```js
for (index = match.index; index < str.length; index++) {
    switch (str.charCodeAt(index)) {
      case 34: // "
        escape = '&quot;';
        break;
      case 38: // &
        escape = '&amp;';
        break;
      case 39: // '
        escape = '&#x27;';
        break;
      case 60: // <
        escape = '&lt;';
        break;
      case 62: // >
        escape = '&gt;';
        break;
      default:
        continue;
    }
  }
```
### 可能的漏洞
#### dangerouslySetInnerHTML
- 直接把用户的输入传递给`dangerouslySetInnerHTML`
- 构造属性时所采用的JSON中包含`dangerouslySetInnerHTML`
  ```js
  // 用户的输入
  const userProvidePropsString = `{"dangerouslySetInnerHTML":{"__html":"<img onerror='alert(\"xss\");' src='empty.png' />"}}"`;
  // 经过 JSON 转换
  const userProvideProps = JSON.parse(userProvidePropsString);
  // userProvideProps = {
  //   dangerouslySetInnerHTML: {
  //     "__html": `<img onerror='alert("xss");' src='empty.png' />`
  //      }
  // };
  render() {
      // 出于某种原因解析用户提供的 JSON 并将对象作为 props 传递
      return <div {...userProvideProps} /> 
  }

  ```
#### javascript: 或 data:
```jsx
const userWebsite = "javascript:alert('xss');";
<a href={userWebsite}></a>
```
# 参考链接
- [极客时间-浏览器工作原理与实践](https://time.geekbang.org/column/article/152807)
- [React中的XSS攻击](https://juejin.cn/post/6874743455776505870)