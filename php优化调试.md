###今天主要总结两种线上调试优化PHP程序的方法，如下：
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

