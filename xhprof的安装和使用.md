###XHProf安装
```
wget http://pecl.php.net/get/xhprof-0.9.2.tgz  
tar zxf xhprof-0.9.2.tgz  
cd xhprof-0.9.2    
cd extension/  
/usr/local/adinf/adweb-1.2/php/5.6.26/bin/phpsize 
./configure  --with-php-config=/usr/local/adinf/adweb-1.2/php/5.6.26/bin/php-config
make && make install  
```
###php.ini中添加
```
extension=xhprof.so
xhprof.output_dir=/www/logs/xhprof  
```

【注意】xhprof.output_dir定义分析日志写入的目录，需要注意该目录的权限，给予需要写入用户相应权限
###使用方法
```  
xhprof_enable();   
//xhprof_enable(XHPROF_FLAGS_NO_BUILTINS); 不记录内置的函数，不加这个参数有时候会报502  
//xhprof_enable(XHPROF_FLAGS_CPU + XHPROF_FLAGS_MEMORY);  同时分析CPU和Mem的开销  
$xhprof_on = true;  
  
/*生产环境可使用：  
if (mt_rand(1, 10000) == 1) {  
   xhprof_enable();  
   $xhprof_on = true;  
} */
 
需要测试性能的代码  

 
if($xhprof_on){  
  $xhprof_data = xhprof_disable();  
  $xhprof_root = '/www/（xhprof的虚拟主机目录）/';  
  include_once $xhprof_root."xhprof_lib/utils/xhprof_lib.php";   
  include_once $xhprof_root."xhprof_lib/utils/xhprof_runs.php";   
  $xhprof_runs = new XHProfRuns_Default();   
  $run_id = $xhprof_runs->save_run($xhprof_data, "hx");   
  $url = $xhprof_root."/xhprof_html/index.php?run=$run_id&source=hx";
}  

```
