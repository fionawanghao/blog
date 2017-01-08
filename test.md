### test

![](/assets/u=734814280,4172228468&fm=21&gp=0.jpg)


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
    
        
        
    
    