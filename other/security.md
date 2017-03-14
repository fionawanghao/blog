#PHP开发中的安全问题

###XSS攻击
####原理：

XSS又叫CSS (Cross Site Script) ，跨站脚本攻击。它指的是恶意攻击者往Web页面里插入恶意html代码，当用户浏览该页之时，嵌入其中Web里面的html代码会被执行，从而达到恶意攻击用户的特殊目的。xss漏洞一般发生于与用户交互的地方。

####危害：

- 恶意弹出消息框，影响用户体验
- 获取用户的cookie或者ajax请求攻击者的服务器窃取用户信息
- 可以使用ajax不断调用被攻击网站的某个接口，严重的可能导致带宽被打满无法正常处理用户的请求，除此之外也可以通过调用被攻击网站的某个接口来实现一些个人利益比如在社交网站伪造数据提高影响力等。


####预防办法：

- 参数验证，只接受用户输入我们期待的值
- 使用htmlspecialchars函数将用户输入的HTML标签实体化


###CSRF攻击
####原理：
CSRF（Cross-site request forgery），中文名称：跨站请求伪造，就是冒充用户发请求。比如：灌水机器人等。

####危害：
- 服务器处理大量虚假的请求数据库储存过多垃圾信息影响服务器处理正常请求的性能
- 以用户的身份做出一些危害用户或他人财产安全泄露用户隐私的行为
####预防办法：
- 暂时的方法：分析日志找到发送请求的ip，禁止访问；
- 验证码：设置机器难以识别的验证码，比如掺杂中文的验证码，拖动的验证码等；
- 设置token验证，基本原理是通过比较隐藏的表单传过来的用户发出请求的时间与收到请求时的时间比较，允许一定范围的延时，防止被破解可以加入秘钥和随机字符串。（time（）->md5(秘钥).time()->rand.substr(md5(rand秘钥),0,10).time()）

###SQL注入

####原理：
SQL Injection：是一种利用未过滤/未审核用户输入的攻击方法，通过把SQL命令插入到Web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。
####例子：
SQL注入一般是利用‘’或者“”拼接SQL语句或者--注释掉原有的SQL语句，减少查询的条件获取更多信息或者尝试猜测表名，一旦知道表名就可以对表做增删改查等各种操作。下面是几个常见的SQL注入的例子：

  1）将查询的where条件变成恒真，
  正常的SQL查询语句(xxx为用户要输入的内容）： 
  ```
  SELECT fieldlist FROM table WHERE fieldname='xxx';
  ```
 如果输入 xxx' or '1'='1,SQL语句就变成了
 ```
  SELECT fieldlist FROM table WHERE fieldname='xxx' or '1'='1'; 
```  
  这样的话where条件就不起作用了。
 
   2）猜字段名(--注释掉后面的内容）
  ```
  SELECT fieldlist FROM table WHERE fieldname = 'xxx' AND userid IS NULL; --';  
  SELECT fieldlist FROM table WHERE fieldname = 'xxx' AND email IS NULL; --'; 
  SELECT fieldlist FROM table WHERE fieldname = 'xxx' AND password IS NULL; --'; 
  SELECT fieldlist FROM table WHERE fieldname = 'xxx' AND loginid IS NULL; --'; 
  ```
  上面的语句如果正常执行的话，就说明我们猜测的字段名字是正确的，否则说明猜错了，继续猜。
  
  3）跟上面同样的道理，利用子查询猜测表名：
 ```
 SELECT fieldlist FROM table WHERE fieldname = 'xxx' AND 1=(SELECT COUNT(*) FROM tabname); --';  
```
根据经验推断表名，进行尝试，我们并不关心表里面有多少条记录而是判断当前的表名是不是我们猜测的这个。

4）除了对表进行增删改查以外实际上还可修改数据库
```
SELECT fieldlist FROM table WHERE fieldname = 'xxx'; DROP TABLE members; --';
```
利用 ; 结束上一条语句，在--注释掉后面的语句执行单行多条语句，可能会恶意修改数据库或者更新某个字段的内容。

####危害：
- 数据库信息泄漏：数据库中存放的用户的隐私信息的泄露。
- 网页篡改：通过操作数据库对特定网页进行篡改。
- 网站被挂马，传播恶意软件：修改数据库一些字段的值，嵌入网马链接，进行挂马攻击。
- 数据库被恶意操作：数据库服务器被攻击，数据库的系统管理员帐户被窜改。
- 服务器被远程控制，被安装后门。经由数据库服务器提供的操作系统支持，让黑客得以修改或控制操作系统。
- 破坏硬盘数据，瘫痪全系统。
####预防办法：
- 参数绑定，预处理。
- 一定要接SQL语句抛出的异常，否则容易泄露表名、字段名、数据库服务器等信息

