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

#### nginx的变量
* $host =>  请求Host，如果客户端请求头中没有host，那么这个变量等于为当前请求提供服务的服务器的名称
* $args => 请求行中的参数
* $is_args => 如果$args已经设置，则该变量的值为"?"，否则为""
* $query_string => 和$args类似
* $content_type => 请求头中的Content-Type
* $content_length => 请求中的content-length
* $remote_addr => 客户端的IP地址
* $binary_remote_addr => 二进制格式的客户端地址
* $remote_port => 客户端的端口号
* $remote_user => 已经经过Auth Basic Module验证的用户名，Auth Basic模块中会用到该变量
* $server_name => 服务器名称
* $server_port => 服务器端口号
* $server_addr => 服务器的地址，通常在一次调用后可以确定，若需要避开系统调用，可以在listen中指出地址，并且使用参数bind
* $server_protocol => 请求使用的协议，通常是HTTP/1.0或HTTP/1.1。
* $uri => 不带请求参数的当前URI，$uri不包含主机名
* $document_uri => 和$uri相同
* $document_root => 当前请求在root指令中指定的值
* $http_cookie => 客户端请求header头中的cookie信息。等于js中的document.cookie
* $http_变量名 => 客户端请求header头中的变量，如$http_referer，$http_user_agent
* $http_user_agent => 客户端agent信息，相当于navigator.userAgent
* $http_referer => 网页来源, 相当于document.referer
* $cookie_变量名 => 客户端请求header头中的cookie中具体变量的值
* $scheme => http方法，比如http或者https，相当于location.protocol
* $request_method => 客户端请求的动作，通常为GET或POST
* $request_uri => 带有请求参数的uri，不包含域名，相当于location.pathname和location.search
* $request_filename => 请求的文件路径，由root或alias指令与URI请求生成
* $request_body => 请求的body主体内容
* $limit_rate => 限制连接速率
* $scheme => http方法，如http或者https
* $time_local => 访问时间和时区
