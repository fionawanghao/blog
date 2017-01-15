



====

### curl

get

post
```
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
```

### uc cache

#### redis 操作

telnet 127.0.0.1 6379

set  key value

get key

#### 从cache中获取数据，如果没有按照现有逻辑执行
/api/roles


key => val

uc_roles_keys => array(
 uc_roles_uid_doaminId
 ...
 ...
 ... 
)

uc_roles_uid_doaminId =>json( [id1, id2, id3, id4....])

uc_roles => array(
 uc_roles_uid_doaminId 
)


uc_role_info_roleId => array(
 name=>xxx
 desc=>xxxx,
 resourceinfo => array(
   array(
    'name' => ''
     id => ''
    resource_url => ''
   )
 )

/api/resources

/api/has_resource


#### 将数据库中的数据同步到mysql 中

/usr/local/adinf/adweb-1.2/php/5.6.26/bin/php uc_init.php 

获取到所有的用户
uc_init.php
domainId 
 uid 
 
roles =>


/usr/local/adinf/adweb-1.2/php/5.6.26/bin/php uc_sync.php 

$redis->lpop('uc_sync_queuq');

策略：
domain -> add      // none
domain -> delete  //


存储到同步的list 数据类型中的数据， 同步队列的key: uc_sync_queue, 数据格式如下

$redis->lPush('uc_sync_queue', jsone_encode($message));

$message = array(
 'type' => 'domain' / 'roles'/ 'user' / 'resource',
 'opt'  => 'update'/ 'delete'/ 'add'
 'data' => array(
   
  )
);

例如：





 



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

刷新权限
flush privileges;
 
192.168.10.224  192.168.10.%


