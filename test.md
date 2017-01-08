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
    
    - 