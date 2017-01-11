###cookie

http协议具有无状态的特性，在session没有出现之前，基本上所有的网站都采用Cookie来跟踪会话。cookie跟session的不同在于，cookie是在客户端记录信息确定用户身份。

- 创建cookie
```
setCookie($cookieName,$value,time()+秒数)；
```
第三个参数为过期时间，如果不写，浏览器关闭则过期
- 取值
```
$_cookie[$cookieName];
```
- 删除cookie
```
setcookie($cookieName,value,time()-秒数)；
setcookie($cookiename, '');
setcookie($cookiename, NULL)
```
-删除所有的cookie
```
foreach($_COOKIE as $key=>$val){
        setcookie($key,"",time()-100);
}
```
###session

session简单的说就是用来在server端记录用户的会话信息，通过session id来辨别是那个用户进行的操作从而判断用户的访问权限等。session id 默认存在cookie，也可以使用其他方式。

- 开启session：
```
Session_start()
```
- 注册SESSION变量：
```
$_SESSION['name']='Lisa';   
$_SESSION['passwd'']='XXXXXXXXX';
$_SESSION['time']=time();
```
- 销毁session
```
unset ($_SESSION['xxx']);
$_SESSION=array();
session_destroy();
```
- 设置session有效期
```
if(!isset($_SESSION['last_access'])||(time()-$_SESSION['last_access'])>60)
        $_SESSION['last_access'] = time();
```
- session垃圾回收机制

   gc_probability/gc_divisor规定了gc进程在每次执行session_start()函数的时候会被调用到的概率。