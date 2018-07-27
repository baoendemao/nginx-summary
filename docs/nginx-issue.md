* nginx: [error] invalid PID number "" in "/usr/local/var/run/nginx/nginx.pid"
    * 解决办法：
    ```
    $ sudo nginx -c /etc/nginx/nginx.conf
    $ sudo nginx -s reload
    ```
*  本地启动nginx失败：bind() to 0.0.0.0:80 failed 
    * 解决方法：
    ```
    netstat -aon | findstr :80    // windows查看是不是80端口被某个服务占用
    net stop http                 // 因为使用http-server启动本地服务引起的, http服务将80端口占用，则stop
    ```
* nginx -s reload不生效
    * 解决方法：
    ```
    ps -ef | grep nginx  => kill nginx的相关进程， 然后sudo nginx -c /etc/nginx/nginx.conf
    ```
* nginx: [emerg] unknown log format "main" in /etc/nginx/nginx.conf
    * 解决方法
    ```
    log_fomat的语法：
    log_format  main  '$remote_addr - [$time_local] "$request" '
                      '$status $body_bytes_sent '

    且nginx.conf文件中只能有一个log_format的格式生效：

    access_log  /var/log/nginx/access.log  main;

    ```
