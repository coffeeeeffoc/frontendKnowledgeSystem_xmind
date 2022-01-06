## yarn版本
yarn默认是1.21版本
可以通过yarn set version berry启用yarn2版本
## yarn set version berry总是报网络连接失败
报错是
```
  info There appears to be trouble with your network connection. Retrying...
  info There appears to be trouble with your network connection. Retrying...
  info There appears to be trouble with your network connection. Retrying...
  error An unexpected error occurred: "https://github.com/yarnpkg/berry/raw/master/packages/yarnpkg-cli/bin/yarn.js: connect EHOSTUNREACH 20.205.243.166:443".
  info If you think this is a bug, please open a bug report with the information provided in "F:\\playground\\apps\\react-app-210711\\yarn-error.log".
  info Visit https://yarnpkg.com/en/docs/cli/policies for documentation about this command.
```
本人采用`https://portal.shadowsocks.nz/index.php`梯子，客户端采用Trojan-Qt5连接。【设置-常规设置-入站设置】，开启http代理，http端口号是58591。
此时，设置yarn的代理为下，即可正常连接：
`yarn config set proxy http://127.0.0.1:58591`
