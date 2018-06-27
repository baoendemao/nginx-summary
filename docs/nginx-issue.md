* nginx: [error] invalid PID number "" in "/usr/local/var/run/nginx/nginx.pid"
    * 解决办法：
    ```
    $ sudo nginx -c /usr/local/etc/nginx/nginx.conf
    $ sudo nginx -s reload
    ```