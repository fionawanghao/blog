# Session 共享

### 共享实现方式

使用 redis 代替原有的文件存储 session 信息实现共享

##### 配置 PHP 的 INI 配置文件实现

- 将 `session.save_handler` 配置项修改为 `session.save_handler = redis`
- 将 `session.save_path` 配置项修改为 `session.save_path = "tcp://127.0.0.1:6379"`, 其中 save_path 配置的是 redis 服务的 IP 和 PORT
- 重启nginx 和 php-fpm, 使用 `phpinfo` 就能看到 session 保存方式和路径发生了改变
- 编写PHP代码

```php
session_start();
$_SESSION['test_session']= @array('name' =>'fiona' , 'ccc'=>'hello redis ');
$redis = new redis();
$redis->connect('127.0.0.1', 6379);
$arg = 'PHPREDIS_SESSION:' .session_id();
var_dump($_SESSION['test_session']);
```

##### 使用 `session_set_save_handler` 函数自定义设置

简单的PHP代码如下：

```php
<?php
class Session_custom {
	private $db; 
	private $prefix;

	public function __construct($prefix = 'sess_', $db) {
		$this->db = $db;	
		$this->prefix = $prefix;
	}

	public function open(){

	}

	public function close() {
		$this->db = null;
		unset($this->db);

	}	

	public function write($session_id, $data) {
		$id = $this->prefix . $session_id;
		$this->db->set($id,json_encode(array('expire' => time(),'data' => $data)));
	}


	public function read($session_id) {
		$id = $this->prefix . $session_id;
		$sessData = $this->db->get($id);
		return $sessData;
	}



	public function destroy($session_id) {
		$this->db->del($this->prefix . $session_id);

	}

	public function gc($maxlifetime) {

	}
} 

$redis = new Redis();
$redis->connect('127.0.0.1',6379);
$SESSION_ID_PREFIX = 'RSID:';
$SESSION_MAX_TIME  = 1440;
ini_set('session.gc_maxlifetime',$SESSION_MAX_TIME);
$sessHandler = new Session_custom($SESSION_ID_PREFIX,$redis);
session_set_save_handler(
		array($sessHandler, 'open'),
		array($sessHandler, 'close'),
		array($sessHandler, 'read'),
		array($sessHandler, 'write'),
		array($sessHandler, 'destroy'),
		array($sessHandler, 'gc')
		);

session_start();
echo session_id()."\n\r";
$__SESSION['name']='fiona';
echo $__SESSION['name']."\n\r";
```
