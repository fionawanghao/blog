####在介绍Nginx 与php交互的原理之前先介绍几个相关的概念

- Nginx (“engine x”) 是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器。
- cgi协议解决不同的语言解释器(如php、Python解释器)与webserver的通信,webserver每收到一个请求，都会去fork一个cgi进程，请求结束再kill掉这个进程。
- fast-cgi实现单个进程可以一次处理多个请求。
- php-fpm即php-Fastcgi Process Manager. php-fpm是 fast-cgi 的实现，并提供了进程管理的功能。 
- 进程包含 master 进程和 worker 进程两种进程。 master 进程只有一个，负责监听端口，接收来自 Web Server 的请求，而 worker 进程则一般有多个(具体数量根据实际需要配置)，每个进程内部都嵌入了一个 PHP 解释器，是 PHP 代码真正执行的地方。

####Nginx如何与Php-fpm结合
Nginx通过反向代理功能将动态请求转向后端Php-fpm。在nginx配置文件中添加server，也可以在另外的文件配置再引入到nginx.conf。基本的配置项如下：
```
server {
    listen       80; #监听80端口，接收http请求
    server_name  www.example.com; #就是网站地址
    root /fiona/project; # 准备存放代码工程的路径
    #路由到网站根目录www.example.com时候的处理
    location / {
        index index.php; #跳转到www.example.com/index.php
        autoindex on;
    }   

    #当请求网站下php文件的时候，反向代理到php-fpm
    location ~ \.php$ {
        include /usr/local/etc/nginx/fastcgi.conf; #加载nginx的fastcgi模块
        fastcgi_intercept_errors on;
        fastcgi_pass   127.0.0.1:9000; #nginx fastcgi进程监听的IP地址和端口
    }
```
当访问www.example.com的时候，流程是这样的：
```
www.example.com
        |
        |
      Nginx
        |
        |
路由到www.example.com/index.php
        |
        |
加载nginx的fast-cgi模块
        |
        |
fast-cgi监听127.0.0.1:9000地址
        |
        |
www.example.com/index.php请求到达127.0.0.1:9000
        |
        |
php-fpm 监听127.0.0.1:9000
        |
        |
php-fpm 接收到请求，启用worker进程处理请求
        |
        |
php-fpm 处理完请求，返回给nginx
        |
        |
nginx将结果通过http返回给浏览器

```
