#Windows安装redis扩展

### 步骤1：

下载合适的版本，根据自己的计算机选择32位或64位:http://pecl.php.net/package/redis 

###步骤2：

解压后将.dll文件拷入到php/ext文件夹

###步骤3：

修改php.ini,将下面的内容添加到php.ini

```
;php_redis
extension=php_redis.dll
```

###步骤4：

重启nginx,phpinfo()看有没有redis扩展（如果没有可以在php.ini 目录下执行 php -m 看是否报错）

