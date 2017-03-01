####下面是利用函数register_shutdown_function和register_shutdown_function捕获error并进行自定义处理的示例，register_shutdown_function可以捕获fatal_error,然后利用回调函数做一些操作。




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


`