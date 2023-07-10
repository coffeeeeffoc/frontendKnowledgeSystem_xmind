[toc]
# CSRF
Cross-site request forgery  跨站请求伪造
**CSRF 攻击就是黑客利用了用户的登录状态，并通过第三方的站点来做一些坏事**

## 发起方式

  ### 自动发起Get请求
  - 利用img标签的src属性
  ### 自动发起Post请求
  ### 引诱用户点击链接

## 发起攻击的必要条件

  ### 目标站点一定要有 CSRF 漏洞
  ### 用户要登录过目标站点，并且在浏览器上保持有该站点的登录状态
  ### 需要用户打开一个第三方站点，可以是黑客的站点，也可以是一些论坛
## 防护手段
主要是提升服务器的安全性
### 充分利用好 Cookie 的 SameSite 属性
SameSite的三个选项
- Strict
  浏览器会完全禁止第三方 Cookie，不同域肯定获取不到当前域的cookie
- Lax
  第三方站点的Get请求能访问当前站点的Cookie，Post请求不会携带
- None
  任何情况都可以携带当前站点的Cookie
### 根据Origin或Referer验证请求的来源站点
- `Origin`
  只包含域名信息
- `Referer`
  包含请求时的来源地址
  由于`Referrer Policy`原因，`Referer`可能存在或者不存在
**所以服务器优先判断Origin，再判断Referer值**
### Token