## npm 配置
命令
```
npm config set proxy http://username:password@host:port
npm config set https-proxy http://username:password@host:port
```
没有用户名和密码，则直接使用http://host:port即可
或者编辑.npmrc文件
```
proxy=http://username:password@host:port
https-proxy=http://username:password@host:port
https_proxy=http://username:password@host:port
```