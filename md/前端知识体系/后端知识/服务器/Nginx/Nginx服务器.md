# Nginx服务器

## 常用命令

### 查看Nginx的版本号：nginx -V

启动Nginx：start nginx

快速停止或关闭Nginx：nginx -s stop

正常停止或关闭Nginx：nginx -s quit

配置文件修改重装载命令：nginx -s reload

查看windows任务管理器下Nginx的进程命令：tasklist /fi "imagename eq nginx.exe"

查看端口$ netstat -an | grep 端口  # linux/mac系统
> netstat -an | findstr 端口  # windows系统

测试配置文件并查看位置：nginx -t

## 配置
https://segmentfault.com/a/1190000012953480

### proxy_pass和rewrite

- rewrite

	- 地址栏会切换
	- 重定向

- proxy_pass

	- 代理
	- 地址栏还是之前的url
	- location /api {
    proxy_pass  http://192.168.1.181/;
}

		- 如果IP后面有/的话，则把/api替换掉
		- 如果IP后面没有/的话，则不会把/api替换掉

### 跨域请求配置

- 配置参数

	- location / {  
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
    add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';

    if ($request_method = 'OPTIONS') {
        return 204;
    }
} 

- 详细解释

	- https://segmentfault.com/a/1190000012550346
	- Access-Control-Allow-Origin

		- 服务器默认是不被允许跨域的。给Nginx服务器配置`Access-Control-Allow-Origin *`后，表示服务器可以接受所有的请求源（Origin）,即接受所有跨域的请求。
		- # *表示允许所有站跨域访问（不安全，建议指定具体允许的域名如：http://b.domain.com:9000（注意格式：http(s):// + domain + port，末尾也不能加/）

	- Access-Control-Allow-Headers 

		- 是为了防止出现以下错误：
Request header field Content-Type is not allowed by Access-Control-Allow-Headers in preflight response.

这个错误表示当前请求Content-Type的值不被支持。其实是我们发起了"application/json"的类型请求导致的

	- Access-Control-Allow-Methods

		- 是为了防止出现以下错误：
Content-Type is not allowed by Access-Control-Allow-Headers in preflight response.

	- 给OPTIONS 添加 204的返回

		- 是为了处理在发送POST请求时Nginx依然拒绝访问的错误
发送"预检请求"时，需要用到方法 OPTIONS ,所以服务器需要允许该方法。

### 负载均衡
参考链接：https://blog.csdn.net/xyang81/article/details/51702900
nginx官网链接：http://nginx.org/en/docs/http/load_balancing.html
- 内置负载策略
  - 三种负载策略
    - `round-robin`轮循（默认）
        Nginx根据请求次数，将每个请求均匀分配到每台服务器
    - `least-connected`最少连接
        `least_conn`指令来指定采用此策略
        将请求分配给连接数最少的服务器。Nginx会统计哪些服务器的连接数最少。
    - `ip-hash`IP Hash
        `ip_hash`指令来指定采用此策略
        绑定处理请求的服务器。第一次请求时，根据该客户端的IP算出一个HASH值，将请求分配到集群中的某一台服务器上。后面该客户端的所有请求，都将通过HASH算法，找到之前处理这台客户端请求的服务器，然后将请求交给它来处理。
  - 参数
    - weight 权重，默认为1。
    - max_fails 最大失败次数，默认为1。超过最大次数后，在fail_timeout时间内，新的请求将不会分配给这台机器。
    - fail_timeout 失败后的无效时间，默认为10秒。某台Server达到max_fails次失败请求后，在fail_timeout期间内，nginx会认为这台Server暂时不可用，不会将请求分配给它
    - backup 备份机，所有服务器挂了之后才会生效
    - down 不可用，标识某一台server不可用。
    - max_conns 最大连接数量，默认为0。超过这个数量，将不会分配新的连接给它。默认为0，表示不限制。
    - resolve 指定域名解析
    ```
    http {
        upstream a {
            server 192.168.0.100:8080 weight=2 max_fails=3 fail_timeout=15;
            server 192.168.0.100:8081 weight=1;
            server 192.168.0.100:8082 backup;
            server 192.168.0.100:8083 down;
            server 192.168.0.100:8084 max_conns=1000;
            server example.com resolve;
        }
    }
    ```
- 第三方负载策略
    貌似都要编译nginx源码来做的
  - fair
  - url-hash
```
upstream apiServer {
    #此处设置指令，指定所采取的负载策略
    #least_conn;
    #ip_hash;
    server 10.0.0.80:5000;  # 如果需要权重加 weight=数字
    server 10.0.0.81:5000;
}
server {
    listen       80;
    server_name  127.0.0.1;

    location /api {
        proxy_pass   http://apiServer;
    }
}
```
### 开启gzip压缩

- http {
    include          mime.conf;
    default_type     application/octet-stream;
    ....

    gzip             on;
    gzip_min_length  10k;
    gzip_comp_level  5;
    gzip_types       text/plain text/css application/x-javascript application/javascript text/javascript;

    server {
        ....
    }

### https

- 复制.pem和.key两种证书到当前server配置同一个目录下
- server {
    listen       443;
    server_name  127.0.0.1;
    ssl on;
    ssl_certificate             my.pem;    # 替换成自己的证书
    ssl_certificate_key         my.key;    # 替换成自己的证书
    ssl_session_timeout         5m;
    ssl_protocols SSLv3 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES256-SHA384:AES256-SHA256:RC4:HIGH:!MD5:!aNULL:!eNULL:!NULL:!DH:!EDH:!AESGCM;
    ssl_prefer_server_ciphers   on;

    location / {
        ...
    }
}

