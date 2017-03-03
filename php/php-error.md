# PHP 错误捕捉

在开发过程一般需要捕捉一些异常或者错误，PHP 提供了 `register_shutdown_function` 和 `set_exception_handler` 获取错误和异常

http://www.jb51.net/article/35056.htm
http://www.cnblogs.com/ncong/p/3913050.html
http://yanue.net/post-99.html

`register_shutdown_function()` 函数可实现当程序执行完成后执行的函数，其功能为可实现程序执行完成的后续操作。程序在运行的时候可能存在执行超时，或强制关闭等情况，但这种情况下默认的提示是非常不友好的，如果使用`register_shutdown_function()`函数捕获异常，就能提供更加友好的错误展示方式，同时可以实现一些功能的后续操作，如执行完成后的临时数据清理，包括临时文件等


```php
register_shutdown_function(a);
set_error_handler(b);
function b($error,$message,$file,$line){
		global $log;
		$log=array();
		switch($error) {
				case E_ERROR: $type='ERROR';
				break;
				case E_WARNING: $type='WARNING';
				break;
				case E_NOTICE: $type='NOTICE';
				break;
				default: $type='Unknown error type ['.$error.']';
				break;
		}
		$log[] = date('Y-m-d H:i:s', time())."\t".$type.': '.$message.' in line '.$line.' of file '.$file.', PHP '.PHP_VERSION.' ('.PHP_OS.')';
		if(function_exists('debug_backtrace')) {
				$backtrace=debug_backtrace();
				for($level=1;$level<count($backtrace);$level++) {
						$message='File: '.$backtrace[$level]['file'].' Line: '.$backtrace[$level]['line'].' Function: ';
						if(isset($backtrace[$level]['class']))
								$message.='(class '.$backtrace[$level]['class'].') ';

						if(isset($backtrace[$level]['type']))
								$message.=$backtrace[$level]['type'].' ';

						$message.=$backtrace[$level]['function'].'(';
						 if(isset($backtrace[$level]['args'])) {
								for($argument=0;$argument<count($backtrace[$level]['args']);$argument++) {
										if($argument>0)
												$message.=', ';
										$message.=serialize($backtrace[$level]['args'][$argument]);
								}
						} $message.=')';
						$log[]=$message;
				}
		}
}
function a(){
		global $log;
		if ($e = error_get_last()) {
				$log[] = $e['message'] . " in " . $e['file'] . ' line ' . $e['line'];
		}
		print_r($log);
		//还可以干一些其他的事情哦，比方说发邮件
}

```
