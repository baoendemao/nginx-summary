* nginx: [error] invalid PID number "" in "/usr/local/var/run/nginx/nginx.pid"
    * 解决办法：
    ```
    $ sudo nginx -c /usr/local/etc/nginx/nginx.conf
    $ sudo nginx -s reload
    ```
*  本地启动nginx失败：bind() to 0.0.0.0:80 failed 
    * 解决方法：
    ```
    netstat -aon | findstr :80    // windows查看是不是80端口被某个服务占用
    net stop http                 // 因为使用http-server启动本地服务引起的, http服务将80端口占用，则stop
    ```