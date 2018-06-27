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
    * 可以通过修改本地hosts文件的方式
    ```
        // windows的hosts文件的路径
        C:\Windows\System32\drivers\etc
    ```