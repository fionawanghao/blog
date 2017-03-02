###今天主要总结一些线上调试优化PHP程序的思路，如下：
###首先定位问题：
####1.XHProf

#####1)灰度部分

在线上环境使用xhprof无疑会影响性能，可以灰度部分访问来检测响应慢的原因
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

####2.PHP慢日志
修改php-fpm.conf文件，启用php慢日志,设置request_slowlog_timeout 和slowlog。

```
slowlog = /log/$pool.log.slow
; The timeout for serving a single request after which a PHP backtrace will be
; dumped to the 'slowlog' file. A value of '0s' means 'off'.
; Available units: s(econds)(default), m(inutes), h(ours), or d(ays)
; Default Value: 0
request_slowlog_timeout = 5s
```

```
tail -f /log/$pool.log.slow

```
上面的命令即可看到执行过慢的php过程
####3.针对大量调用接口的情况可以统计调用各个接口的耗时然后记录到日志然后分析日志，将数据统计到数据库然后用ECharts画出统计表进行分析。

###查找问题原因并解决问题：
- 响应慢一般是连接了数据库或者调用接口等，先排查网络原因：

  1）用dig命令查看DNS域名解析时间
  
  2）ping一下，看看网络是不是慢
- 如果不是网络原因，数据库的话查看 sql语句和索引的问题，用explain看有没有命中索引，调用别人接口的话反映给接口的负责人，进一步查看接口响应慢的原因

- 请求响应慢，也可能是跟响应无关的程序（如记录日志等）耗时过多，使用fastcgi_finish_request()函数冲刷(flush)所有响应的数据给客户端并结束请求， 这使得客户端结束连接后，需要大量时间运行的任务能够继续运行，且不影响响应客户端的时间。

```
       echo '例子：';
 
       file_put_contents('log.txt', date('Y-m-d H:i:s') . " 上传视频\n", FILE_APPEND);
 
       fastcgi_finish_request();
 
       sleep(1);
       file_put_contents('log.txt', date('Y-m-d H:i:s') . " 转换格式\n", FILE_APPEND);
 
       sleep(1);
       file_put_contents('log.txt', date('Y-m-d H:i:s') . " 提取图片\n", FILE_APPEND);
 
 ```


