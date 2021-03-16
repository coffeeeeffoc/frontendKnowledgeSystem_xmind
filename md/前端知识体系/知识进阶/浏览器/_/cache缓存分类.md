[toc]
# cache缓存分类

## 浏览器缓存

### HTTP缓存

- 强制缓存

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

	- 流程

		- 

	- Etag和If-None-Match（优先级高于Last-Modified/If-Modify-Since）

		- 第一次请求时，由服务端生成
		- ETag可以保证每一个资源是唯一的，是唯一标识符
		- 如果需要校验时，客户端会通过If-None-Match头将先前服务器端发送过来的Etag发送给服务器，服务会对比这个客户端发过来的Etag是否与服务器的相同，若相同，就将If-None-Match的值设为false，返回状态304，客户端继续使用本地缓存，不解析服务器端发回来的数据，若不相同就将If-None-Match的值设为true，返回状态为200，客户端重新机械服务器端返回的数据；客户端还会通过If-Modified-Since头将先前服务器端发过来的最后修改时间戳发送给服务器，服务器端通过这个时间戳判断客户端的页面是否是最新的，如果不是最新的，则返回最新的内容，如果是最新的，则返回304，客户端继续使用本地缓存。
		- 与Last-Modified不一样的是，当服务器返回304 Not Modified的响应时，由于ETag重新生成过，response header中还会把这个ETag返回，即使这个ETag跟之前的没有变化。

	- Last-Modify/If-Modify-Since

		- 浏览器第一次请求一个资源的时候，服务器返回的header中会加上Last-Modify，Last-modify是一个时间标识该资源的最后修改时间，例如Last-Modify: Thu,31 Dec 2037 23:59:59 GMT。

当浏览器再次请求该资源时，request的请求头中会包含If-Modify-Since，该值为缓存之前返回的Last-Modify。服务器收到If-Modify-Since后，根据资源的最后修改时间判断是否命中缓存。

如果命中缓存，则返回304，并且不会返回资源内容，并且不会返回Last-Modify。

### 本地缓存

- indexDB
- cookie

	- 

- WebStorage

	- LocalStorage
	- SessionStorage

- WebSql
- Application Cache
- PWA

### 默认缓存

- 往返缓存BFCache

### H5离线存储manifest

## 服务器缓存

### CDN缓存

### 代理服务器缓存

## 数据库缓存

