# nginx 反向代理

### 反向代理配置

nginx除了实现 web server 的功能外，还可以做反向代理，类似于负载均衡,实现反向代理的基本配置如下

```
#nginx.conf

worker_processes  1;
events {
        worker_connections  1024;
}
 
http {
        keepalive_timeout  65;
    include upstream.conf;
}
```

```
#vim upstream.conf

upstream debugo_servers {
	server ip:port weight=5; //这里写接nginx反向代理过来请求的ip和端口
	server ip:port weight=10;
}
server {
	listen debugo01:80;
	location / {
	proxy_pass http://debugo_servers;
	proxy_set_header   Host             $host;
	proxy_set_header   X-Real-IP        $remote_addr;

	}
 }
```

### 轮询策略

nginx反向代理默认是轮询的方式分配，请求也可根据后端服务器的性能来决定分配的请求的权重。除此之外下面几种方式：

#### IP Hash

需要在upstream的配置中指定“ip_hash;”，每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。

#### fair

要在upstream的配置中指定“fair;”按后端服务器的响应时间来分配请求，响应时间短的优先分配。

#### URL hash

要在upstream中配置指定：“hash $request_uri; hash_method crc32;”按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。


另外，在nignx中可以为每个 backend server 指定最大的重试次数和重试时间间隔。所使用的关键字是 `max_fails` 和 `fail_timeout`, 而backup指的是仅当其他serverdown或者忙时才会请求这台server。例如：

```
upstream debugo_servers {
	server debugo01:80 weight=5 backup;
	server debugo02:80 weight=10 max_fails=3 fail_timeout=30s;
}
```
