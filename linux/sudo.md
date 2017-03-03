#sudo控制执行权限

1.确认sudo软件的安装

2.通过sudo授权用户可以修改配置文件和管理服务脚本：

```
[root@localhost ~]# visudo
```

做如下的修改：
```
helen localhost=/bin/vi /etc/httpd/conf/http.conf,
/etc/rc.d/init.d/httpd start,/etc/rc.d/init.d/httpd reload,
/etc/rc.d/init.d/httpd configtest,/etc/rc.d/init.d/httpd status
```

以上为授权用户helen可以修改apache配置文件，可以启动apache、修改apache配置文件后加载生效、以及检测apache配置文件语法错误和查看apache状态。

####也可以通过chown命令修改文件的所有者

```
chown helen /etc/http/conf/httpd.conf
```
