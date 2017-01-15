



====

### curl



### uc cache

### uc web

### uc sdk


### linux 环境

 fpm 地址目录
 
/usr/local/adinf/adweb-1.2/php/5.6.26/sbin

重新加载fpm php 配置文件
kill -USR2 【fpm 进程号】

nginx 安装目录

/usr/local/adinf/adweb-1.2/ngx_openresty/1.9.3.2/nginx

conf/http/ 配置项目

sbin/nginx 启动nginx 服务

./sbin/nginx -s reload


#### mysql 远程授权

GRANT ALL PRIVILEGES ON [授权的数据库名称].[授权的数据表名称] TO '[访问的用户名]'@'[访问的IP地址，如果是全部IP或者全部IP段使用%代替]' identified by '[密码]' WITH GRANT OPTION  

GRANT ALL PRIVILEGES ON *.* TO 'wp'@'%' identified by '123456' WITH GRANT OPTION  

192.168.10.224  192.168.10.%


