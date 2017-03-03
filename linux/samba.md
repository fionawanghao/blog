# Samba安装和配置

####Samba介绍
Samba服务器可以实现Linux与Linux之间或者Linux与windows的文件共享和打印共享，后者用的的较多。Samba有两个服务，分别是smb(监听139 TCP端口,提供资源共享访问）和nmb(监听137和138 UDP端口,负责NetBIOS主机名称的解析）。
####Samba安装命令

```
[root@localhost ~]# yum install -y samba* 
```

####启动Smaba服务
```
[root@localhost ~]# service smb start
[root@localhost ~]# service nmb start
```
####查看Smaba服务状态
```
[root@localhost ~]# service smb status
```
####设置开机自动启动
```
[root@localhost ~]# chkconfig smb --level 35 on
[root@localhost ~]# chkconfig --list|grep smb
         smb            0:off	1:off	2:off	3:on	4:off	5:on	6:off
```
####Smaba配置
- 打开配置文件
```
[root@localhost ~]# vim /etc/samba/smb.conf
```
- 分别针对共享资源和共享目录修改配置文件，其中server string设定 Samba Server 的注释，可以是任何字符串，也可以不填；security设置用户访问Samba Server的验证方式。security一共有四种验证方式：share，访问不需要用户名密码；user 指定可以访问的用户；server，一种代理验证；domain，使用主域控制器(PDC)来完成认证。根据下面的配置，共享目录访问地址为本机ip地址加上web(可以自行修改），即//192.168.1.108/web。

``` 
[global]
workgroup = FIONA
netbios name = LinuxFiona
server string = Linux Samba Server TestServer
security = share

[web]
path=/fiona/project
writeable=yes
browseable=yes
guest ok=yes
```
####建立共享目录
设置的共享目录需要自己建立该目录 
```
[root@localhost ~]# mkdir -p /hhh/vvv 
```       
###重启Samba服务
```
[root@localhost ~]# service smb restart 
[root@localhost ~]# service nmb restart
```
####linux系统挂载smaba共享目录
```
[root@localhost ~]# mount //192.168.1.108/web /mnt -o username=user1,password=pw
```
####Windows系统访问方式
在开始输入执行，然后输入访问地址//192.168.1.108/web，如图，点击确定就可以看到共享文件了^^

![](/assets/QQ截图20170108195240.png)

####注意
如果访问失败，需要检查防火墙和SELINUX是否关闭。
