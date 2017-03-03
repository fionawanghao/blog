# PHP 接口性能优化

对于 PHP 接口性能优化思路不同的业务有不同的方式，但是排查思路和解决问题的思路基本雷同，遇到性能瓶颈首先要定位问题，找到瓶颈点，下一步根据瓶颈点进行优化

### 定位问题

#### xhprof

通过 xhprof 分析调用链中的耗时的部分，从而定位问题，但是由于 xhprof 会影响性能，所以在大流量的线上业务如果要定位问题可以通过如下两种方式避免影响线上业务性能

##### 灰度部分

在线上环境使用xhprof无疑会影响性能，可以灰度部分访问来检测响应慢的原因, 尽可能的减少受影响的请求

```
if(rand(1,1000）== 1）{
    xhprof_enable();  
} 
```

##### debug 开关

通过在接口加开关 debug=1, 在调试的开启，这样仅仅影响开发人员调用的这次请求，但是这种也有局限性就是样本不够多，对于部分问题可能难以复现

```
if(isset($__GET['debug']) && $__GET['debug'] == 1）{
    xhprof_enable();  
} 
```

#### PHP 慢日志

开启慢日志，开启方式如下:

修改 `request_slowlog_timeout` 和 `slowlog` 参数

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

通过查看慢日志来排查接口性能瓶颈点

#### 统计分析打点日志

针对大量调用接口的情况可以统计调用各个接口的耗时然后记录到日志然后分析日志，将数据统计到数据库然后用ECharts画出统计表进行分析。

### 解决瓶颈点

#### 对于网络请求

- 首先确认 DNS 解析问题

```
#dig domain
#dig www.baidu.com
```

- 其次确认网络环境是否稳定

通过 `ping` 查看是否有丢包的情况

#### 数据库存储读取慢

如果是数据库存储读取慢，可以优化 sql 语句或者加索引，将可以缓存的数据通过加缓存的方式提升性能

#### 业务逻辑问题

跟响应无关的程序（如记录日志等）耗时过多，使用fastcgi_finish_request()函数冲刷(flush)所有响应的数据给客户端并结束请求， 这使得客户端结束连接后，需要大量时间运行的任务能够继续运行，且不影响响应客户端的时间。

```php
echo '例子：';

file_put_contents('log.txt', date('Y-m-d H:i:s') . " 上传视频\n", FILE_APPEND);

fastcgi_finish_request();

sleep(1);
file_put_contents('log.txt', date('Y-m-d H:i:s') . " 转换格式\n", FILE_APPEND);

sleep(1);
file_put_contents('log.txt', date('Y-m-d H:i:s') . " 提取图片\n", FILE_APPEND);

```


