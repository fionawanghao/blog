# Mysql 远程授权

### 应用场景

远程访问服务器的MySQL数据库，又不想公布root账号，可以授权给某个用户在某个ip或者ip段访问服务器的MySQL数据库中的某个数据库。

####授权方法：

#####授权的基本语法：
```
mysql>GRANT [权限] ON [授权的数据库名称].[授权的数据表名称] TO '[访问的用户名]'@'[访问的IP地址，如果是
全部IP或者全部IP段使用%代替]' identified by '[密码]' WITH GRANT OPTION  
```

```
mysql>flush privileges;//刷新系统权限表
```
####示例

###### 创建用户fiona

```
mysql> insert into mysql.user(Host,User,Password) values("localhost","fiona",password("1234"));
```

###### 查看所有用户

```
mysql> SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
```

###### 授权fiona用户拥有fionaDB数据库的所有权限

```
mysql>grant all privileges on fionaDB.* to fiona@% identified by '1234';
```

```   
mysql>flush privileges;
``` 

###### 指定部分权限给一用户

```
mysql>grant select,update on fionaDB.* to fiona@% identified by '1234';
```

```
mysql>flush privileges; 
```

###### 授权fiona用户拥有所有数据库的某些权限： 　 

```
mysql>grant select,delete,update,create,drop on *.* to fiona@"%" identified by "1234";
```

###### 删除用户

```
mysql> Delete FROM mysql.user Where User='fiona' and Host='localhost';
```

```
mysql>flush privileges;
```

###### 修改指定用户密码

```
mysql>update mysql.user set password=password('新密码') where User="test" and Host="localhost";
```

```
mysql>flush privileges;
```
 
