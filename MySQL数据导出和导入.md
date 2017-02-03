###MySQL数据导出

首先进入在mysql的bin目录执行
```
mysqldump -u 用户名 -p 数据库名 > 导出的文件名 
```
###MySQL数据导入
- 登录数据库
```
mysql -u root -p 
```
- 进入你要导入数据的数据库
```
mysql>use 数据库 
```
- 导入
```
mysql>source 要导入的数据目录 
```
【注意】导入时，目录的冒号后面的第一个目录分割符去掉，否则会报错如D:\wamp\bin\mysql\mysql5.6.12\bin\aaa 要写成
D:wamp\bin\mysql\mysql5.6.12\bin\aaa