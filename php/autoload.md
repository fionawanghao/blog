# PHP自动加载及包管理

### autoload

##### 魔术方法__autoload()

###### 作用

PHP面向对象开发中常常把重复使用的类放在一个或几个文件中，使用的时候引用这些文件来包含需要的类。__autoload可以自动加载需要的类的文件，这样没有使用到的类文件就不会被引用，以提高效率，代码也更加整洁。

###### 原理
当使用到本文件中没有声明的类时，就会调用__autoload()，系统会按照该方法中定义的规则自动加载需要的文件。

###### 实例

```php
function __autoload($class_name) { 
    $path = str_replace('_', '/', strtolower($class_name)); 
    require_once $path . '.php'; 
}
       
// 当调用本文件不存在的类Project_Interface，会自动加载project/interface.php 文件 
$a = new Project_Interface(); 

```

##### spl_autoload*系列函数

当多个—__autoload（）同时使用时会发生冲突，spl的autoload系列函数可以解决这个问题。

###### 实例1

```php
set_include_path("project/test/demo"); //这里需要将路径放入include 
spl_autoload("http"); 
$a = new http(); //寻找/project/test/demo/http.php 
```

###### 实例2

```php
spl_autoload_register(function($class){ 
if($class == 'http'){ 
require_once("project/test/demo/http.php "); 
} 
}); 
$a = new http(); 
```

###依赖管理工具composer

composer是一种目前非常流行的包依赖管理工具，安装完成以后写一个composer.json文件配置需要依赖的包名称和版本，格式如下：

```javascript
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

然后执行composer install 或者composer update，会自动生成vendor/autoload.php将该文件引入，就可以使用你需要库文件了。

那么这个是怎样实现的呢？composer生成了一个autoloader，根据各个包自己的autoload配置，从而帮我们完成进行自动加载的工作。

#### 自定义自动加载方式

对于第三方的自动加载，composer提供了四种方式。分别是PSR-0和PSR-4的自动加载,class-map，和直接包含files的方式。

##### PSR-4自动加载

```javascript
{
  "autoload": {
    "psr-4": {
      "Foo\\": "src/",
    }
  }
}
```

当试图自动加载 "Foo\\Bar\\Baz" 这个class时，会去寻找 "src/Bar/Baz.php" 这个文件.在composer安装或更新完之后，psr-4的配置换被转换成namespace为key，dir path为value的Map的形式，并写入vendor/composer/autoload_psr4.php 文件之中。

##### PSR-0自动加载

```javascript
{
  "autoload": {
    "psr-0": {
      "Foo\\": "src/",
    }
  }
}
```

按照PSR-0的规则，当试图自动加载 "Foo\\Bar\\Baz" 这个class时，会去寻找 "src/Foo/Bar/Baz.php"这个文件。最终这个配置也以Map的形式写入vendor/composer/autoload_namespaces.php

##### Class-map方式

```javascript
{
  "autoload": {
    "classmap": ["src/", "lib/", "Something.php"]
  }
}
```

Class-map方式，则是通过配置指定的目录或文件，然后在Composer安装或更新时，它会扫描指定目录下以.php或.inc结尾的文件中的class，生成class到指定file path的映射，并加入新生成的 vendor/composer/autoload_classmap.php 文件中，。

##### Files方式(手动指定供直接加载的文件)

```javascript
{
  "autoload": {
    "files": ["src/MyLibrary/functions.php"]
  }
}
```

Files方式会生成一个array，包含这些配置中指定的files，再写入 vendor/composer/autoload_files.php
