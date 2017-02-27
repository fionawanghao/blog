###今天主要总结线上调试优化PHP程序的方法，如下：
####1.fastcgi_finish_request()

此函数冲刷(flush)所有响应的数据给客户端并结束请求。 这使得客户端结束连接后，需要大量时间运行的任务能够继续运行。
```
echo '例子：';
 
file_put_contents('log.txt', date('Y-m-d H:i:s') . " 上传视频\n", FILE_APPEND);
 
fastcgi_finish_request();
 
sleep(1);
file_put_contents('log.txt', date('Y-m-d H:i:s') . " 转换格式\n", FILE_APPEND);
 
sleep(1);
file_put_contents('log.txt', date('Y-m-d H:i:s') . " 提取图片\n", FILE_APPEND);
 
 ```


####2.XHProf

#####1)灰度部分

在线上环境使用xhprof无意会影响性能，可以灰度部分访问来检测响应慢的原因
```
if(rand(1,1000）== 1）{
    xhprof_enable();  
} 
```

#####2) 接口加开关 debug=1
也可以加一个开关，在响应慢的接口使用xhprof。
```
if(isset($__GET['debug']) && $__GET['debug'] == 1）{
    xhprof_enable();  
} 
```

####3.PHP慢日志
修改php-fpm.conf文件，启用php慢日志,设置request_slowlog_timeout 和slowlog。

```
slowlog = log/$pool.log.slow
; The timeout for serving a single request after which a PHP backtrace will be
; dumped to the 'slowlog' file. A value of '0s' means 'off'.
; Available units: s(econds)(default), m(inutes), h(ours), or d(ays)
; Default Value: 0
request_slowlog_timeout = 5s
```