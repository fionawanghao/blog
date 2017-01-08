### test

![](/assets/u=734814280,4172228468&fm=21&gp=0.jpg)

- 两天一篇博客
    
    ssession cookie 相关内容
        session共享
        cookie 跨域
    smb 服务相关
    接口认证相关内容
    RBAC基于角色的统一权限认证
        实用redis cache 优化 uc 接口性能
        cache 实时同步的方式
    SSO 统一登录认证
    
    如何使用 xprof 优化 php 性能
    
    如何优化mysql
        - 语句层面 explain 慢查询
        - mysql 服务层面（选用）innodb 优化
        
    
    

### 登录相关

- 页面

    - /login/index 登录页面
    
- 接口实现
    
 - /login/verfity  当用户点击登录的时候提交请求的接口 API
    
    - username, password(明文) 
    - 转化密码为加密后的，并且获取用户信息进行匹配
    - 如果成功直接 设置对应的 session 并且跳转到 refer页面     
    - $_SESSION['is_login'] = 1;
    - $_SESSION['username'] = 'sssss';
            
    - 封装一个yaf插件，在preDispatch这个过程中验证用户是否登录
        - 检查 $_session['is_login'] 是否存在并且为1，如果是返回true
        - 如果没有或者不为1则跳转到header('Location: domain.com/login/index')
        
    
- 数据库
    
        user_info 
        id 
        name
        passwd md5(明文密码 + '123456');
        status
        is_delete
        create_time
        update_time
    
 
 ### UC 改进部分
 
 
 - 对外接口增加 cache 功能
 
     - 暂时选用 redis key=>val 格式存储
     - 需要解决 cahce 同步问题
     
    