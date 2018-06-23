#### http{}
* log_format
    * 只能配置在http块下
    * 
        ```
         log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

        ```
* include
    * nginx读取nginx.conf的时候，同时会读取conf.d目录下面的所有以.conf结尾的所有文件

    ```
    include /etc/nginx/conf.d/*.conf    
    ```

#### server{} => server块对应一个站点的服务
* listen 端口号
* server_name 域名
    * 默认值是localhost。设置访问的域名，多个用空格分开。如www.xxx.com www.yyy.com。
* error_page
    * 错误页面
    ```
    error_page 500 502 404 /50x.html
    ```
    
* set $api_url

* 逻辑控制语句 
    * if
    * 没有else
    * 没有&&

* server块中常用的匹配
    * ~  大小写匹配
    * !~  区分大小写不匹配

    * =   等于，严格匹配， location = xxx
    * !=  不等于，严格匹配
    * ~*  不区分大小写匹配
    * !~* 不区分大小写不匹配
    *  \* 任意字符
    * -f和!-f 判断是否存在文件
    * -d和!-d 判断是否存在目录
    * -e和!-e 判断是否存在文件或目录
    * -x和!-x 判断文件是否可执行

* location{} => location块对应一个站点的路由 => 一个站点可以有多个location
    * location匹配规则遵从上面的server块的匹配规则
        * 例如： location ~ ^/list.html {}
    * proxy_pass
        * proxy_pass用来设置反向代理
    * root
        * root用来设置静态目录
    * random_index on|off
        ```
            location / { 
                root   /var/www/www.xxx.com;
                # 随机选择该目录下的页面作为主页
                random_index on;
            } 
        ```

#### nginx的变量
* $host =>  请求Host
* $args => 请求行中的参数
* $content_type => 请求头中的Content-Type
* $content_length => 请求中的content-length
* $remote_addr => 客户端的IP地址
* $remote_port => 客户端的端口号
* $remote_user => 已经经过Auth Basic Module验证的用户名
* $server_name => 服务器名称。
* $server_port => 服务器端口号
* $server_addr => 服务器的地址，通常在一次调用后可以确定，若需要避开系统调用，可以在listen中指出地址，并且使用参数bind
* $server_protocol => 请求使用的协议，通常是HTTP/1.0或HTTP/1.1。
* $http_user_agent => 客户端agent信息，相当于navigator.userAgent
* $http_referer => 网页来源, 相当于document.referer
* $uri => 不带请求参数的当前URI，$uri不包含主机名
* $document_uri => 和$uri相同
* $document_root => 当前请求在root指令中指定的值
* $request_uri => 带有请求参数的uri，不包含主机名，相当于location.pathname和location.search
* $http_cookie => 客户端cookie信息。等于js中的document.cookie
* $scheme => http方法，比如http或者https，相当于location.protocol
* $request_method => 客户端请求的动作，通常为GET或POST
* $limit_rate => 限制连接速率
* $request_filename => 请求的文件路径，由root或alias指令与URI请求生成

#### error_log 
* nginx的错误日志，默认为/var/log/nginx/error.log  
```
error_log /var/log/nginx/error.log;

```
#### access_log
* 每次浏览器http请求nginx上的一个资源，都会多出来一条log日志
```
access_log /var/log/nginx/access.log main;
```

#### worker_processes
* 工作进程数，默认值auto
* 一般与cpu数目一致，防止cpu切换造成的性能损耗

#### pid
* 存放nginx pid的位置，默认为/var/run/nginx.pid

#### events
* worker_connections，默认值为1024
* 一个进程允许处理的最大连接数

#### 跨域
* 从http响应报文中的access-control-allow-origin可以看出服务器端是否允许跨域请求
* nginx例子
```
location ~ .*\.(html|htm)$ {
    add_header Access-Control-Allow-Origin http://www.bbb.com # nginx服务端允许浏览器http跨域请求www.bbb.com，即只允许这一个域名。
    # 如果配置* ，即允许跨站访问所有的
    add_header Access-Control-Allow-Methods GET,POST,PUT,DELETE,OPTIONS  # 控制http请求的方法类型
    root /var/www/pages;
}

```

#### nginx负载均衡
* upstream server
    * 需要配置在server{}之外
* 如何配置upstream？
```
// 每次请求分流到不同的服务器。如果其中某个server down了，或者端口被关(可以通过iptables设置)，负载均衡会检测到并分流到其他的server。
upstream xxxname {
    server ip1:12800;
    server ip2:12801;
    server ip3:12802;
}
```

#### http迁移到https
```
    location / {
        return 302 https://$host$request_uri;
    }

```