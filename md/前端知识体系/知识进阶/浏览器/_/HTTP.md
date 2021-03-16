# HTTP

## 缓存

### 两大分类

- 强缓存

	- Expires（HTTP 1.0）

		- 服务器返回的到期时间
		- 存在的问题

			- 到期时间是服务端的时间，与客户端时间有误差
			- 解决办法

				- 配合Cache-Control:max-age=XXX，优先级高于Expires

	- Cache-Control（HTTP 1.1）

		- 常见取值

			- private

				- 客户端可以缓存

			- public

				- 客户端和代理服务器都可缓存

			- no-cache

				- 需要使用比对缓存来验证缓存数据

			- max-age

				- 缓存的内容将在 xxx 秒后失效

			- no-store

				- 所有内容都不会缓存

- 协商缓存

	- Etag和If-None-Match（优先级高于Last-Modified/If-Modify-Since）

		- 第一次请求时，由服务端生成
		- ETag可以保证每一个资源是唯一的，是唯一标识符
		- 如果需要校验时，客户端会通过If-None-Match头将先前服务器端发送过来的Etag发送给服务器，服务会对比这个客户端发过来的Etag是否与服务器的相同，若相同，就将If-None-Match的值设为false，返回状态304，客户端继续使用本地缓存，不解析服务器端发回来的数据，若不相同就将If-None-Match的值设为true，返回状态为200，客户端重新机械服务器端返回的数据；客户端还会通过If-Modified-Since头将先前服务器端发过来的最后修改时间戳发送给服务器，服务器端通过这个时间戳判断客户端的页面是否是最新的，如果不是最新的，则返回最新的内容，如果是最新的，则返回304，客户端继续使用本地缓存。
		- 与Last-Modified不一样的是，当服务器返回304 Not Modified的响应时，由于ETag重新生成过，response header中还会把这个ETag返回，即使这个ETag跟之前的没有变化。

	- Last-Modify/If-Modify-Since

		- 浏览器第一次请求一个资源的时候，服务器返回的header中会加上Last-Modify，Last-modify是一个时间标识该资源的最后修改时间，例如Last-Modify: Thu,31 Dec 2037 23:59:59 GMT。

当浏览器再次请求该资源时，request的请求头中会包含If-Modify-Since，该值为缓存之前返回的Last-Modify。服务器收到If-Modify-Since后，根据资源的最后修改时间判断是否命中缓存。

如果命中缓存，则返回304，并且不会返回资源内容，并且不会返回Last-Modify。

### 如何设置缓存

- HTML Meta标签控制缓存（非HTTP协议定义）

	- <META HTTP-EQUIV="Pragma" CONTENT="no-cache">

	- 上述代码的作用是告诉浏览器当前页面不被缓存，每次访问都需要去服务器拉取。这种方法使用上很简单，但只有部分浏览器可以支持，而且所有缓存代理服务器都不支持，因为代理不解析HTML内容本身。

- HTTP头信息控制缓存

	- 强缓存
	- 协商缓存

### 缓存流程

- 第一次请求

	- 

- 第二次请求

	- 

### 强制缓存和协商缓存的关系

- 两种缓存规则可以只有一个，也可以同时存在
- 两种规则并存时，强制缓存规则高于协商缓存

### 缓存刷新

- 浏览器缓存刷新

	- 强缓存、协商缓存

		- 地址栏输入url回车
		- 链接跳转
		- 前进、后退

	- 仅协商缓存

		- F5
		- 浏览器刷新按钮

	- 完全不使用缓存

		- Ctrl + F5
		- Ctrl + 点击浏览器刷新按钮

	- 用户行为与缓存

		- 

- CDN缓存刷新

	- CDN节点对开发者时透明的，可以通过CDN服务商提供的“刷新缓存”接口来达到清理CDN节点缓存的效果，强制使数据过期，从而获取到最新的数据
	- 

### HTTP缓存体系

- 参考链接
https://mp.weixin.qq.com/s/qOMO0LIdA47j3RjhbCWUEQ?utm_source=caibaojian.com
- 缓存存储策略
- 缓存过期策略
- 缓存比对策略

