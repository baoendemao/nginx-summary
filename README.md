# nginx-summary 
* nginx原理
    * http反向代理服务器
    * 静态资源web服务
    * 负载均衡调度器SLB
    * 动态缓存
    * nginx的优点
        * IO多路复用epoll
        * 代码模块化
* nginx相关文件
    * /etc/nginx  => 配置文件目录
    * /etc/nginx/nginx.conf   => 总的配置文件
    * /var/log/nginx   => nginx log
    * /usr/sbin/nginx   => which nginx
* [nginx相关命令](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-command.md)
* [nginx conf配置文件详解](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-conf.md)
* [配置示例](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-eg.md)
* [常见问题解决](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-issue.md)
* nginx本地测试
    * 对于域名的测试可以通过修改本地hosts文件的方式
    ```
        // windows的hosts文件的路径
        C:\Windows\System32\drivers\etc
    ```
    * 改变nginx文件后，要使用nginx -t检测nginx配置文件是否正确
    * 对于windows的用户，可以将nginx.exe安装为系统服务运行，以便于每次开机自启动。安装方法：
        * 下载"Windows Service Wrapper"，放在nginx安装目录中，并重命名为myapp.exe。
        * 新建myapp.xml
        ```
        <service>    
            <id>nginx</id>    
            <name>nginx</name>    
            <description>nginx</description>    
            <executable>D:/nginx/nginx.exe</executable>    
            <logpath>D:/nginx/logs</logpath>    
            <logmode>roll</logmode>    
            <depend></depend>    
            <startargument>-p D:/nginx</startargument>    
            <stopargument>-p D:/nginx -s stop</stopargument>    
        </service>    
        ```
        * 执行nginx.exe install, 在系统服务中看到nginx服务就说明配置成功
        * 之后可以在cmd中使用net start/stop nginx来启动关闭nginx