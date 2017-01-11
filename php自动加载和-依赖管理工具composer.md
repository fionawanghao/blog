###魔术方法__autoload()
####作用：
PHP面向对象开发中常常把重复使用的类放在一个或几个文件中，使用的时候引用这些文件来包含需要的类。__autoload可以自动加载需要的类的文件，这样可以没有使用到的类文件就不会被引用，以提高效率，代码也更加整洁。
####原理：
当使用到本文件中没有声明的类时，就会调用__autoload()，系统会按照该方法中定义的规则自动加载需要的文件。
####实例：
实例1：
```
function __autoload($class_name) { 
    $path = str_replace('_', '/', strtolower($class_name)); 
    require_once $path . '.php'; 
}
       
// 当调用本文件不存在的类Project_Interface，会自动加载project/interface.php 文件 
$a = new Project_Interface(); 
```
实例2：
```
$map = array( 
'Http_File_Interface' => 'C:/PHP/HTTP/FILE/Interface.php' 
); 
function __autoload($class_name) { 
if (isset($map[$class_name])) { 
require_once $map[$class_name]; 
} 
} 
// 这里会自动加载C:/PHP/HTTP/FILE/Interface.php 文件 
$a = new Http_File_Interface(); 
```