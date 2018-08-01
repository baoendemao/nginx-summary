#### nginx的log文件
* error_log 
    * nginx的错误日志，默认为/var/log/nginx/error.log  
    ```
    error_log /var/log/nginx/error.log;

    ```
* access_log
    * 每次浏览器http请求nginx上的一个资源，都会多出来一条log日志
    ```
    access_log /var/log/nginx/access.log main;
    ```

#### log_format
* 只能配置在http块下， 例如：
    ```
        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

        // 每条访问记录会以上面的格式出现在access.log中
        access_log  /var/log/nginx/access.log  main;
    ```
    
* 将log配置成Json格式
```
log_format log_json '{ "@timestamp": "$time_local", '
                         '"@fields": { '
                         '"remote_addr": "$remote_addr", '
                         '"remote_user": "$remote_user", '
                         '"time_local": "$time_local", '
                         '"http_host": "$http_host", '
                         '"body_bytes_sent": "$body_bytes_sent", '
                         '"request_time": "$request_time", '
                         '"request_body": "$request_body", '
                         '"status": "$status", '
                         '"upstream_status": "$upstream_status", '
                         '"upstream_addr": "$upstream_addr", '
                         '"upstream_response_time": "$upstream_response_time", '
                         '"request": "$request", '
                         '"request_method": "$request_method", '
                         '"request_time": "$request_time", '
                         '"http_referrer": "$http_referer", '
                         '"body_bytes_sent":"$body_bytes_sent", '
                         '"http_x_forwarded_for": "$http_x_forwarded_for", '
                         '"http_user_agent": "$http_user_agent" } }';

access_log  /var/log/nginx/access.log  log_json;

```

