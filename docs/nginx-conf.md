#### nginx的conf文件结构
```
# 全局块
   ...
   
# events块
events {
    ...
}

# http块
http {
    # server块
    server {
        # location块
        location {
            ...
        }
        # location块
        location {
            ...
        }
    }

    # server块
    server {
        # location块
        location {
            ...
        }
    }
}
```
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

#### server{} => server块对应一个站点的服务 => 一个server块是一个虚拟主机 => 在一台机器上配置多个虚拟主机，只需要配置多个server块
* listen 监听的ip和端口号，也可以只写端口号
* server_name 主机名称
    * 基于ip的虚拟主机
    * 基于域名的虚拟主机
        * 默认值是localhost。设置访问的域名，多个用空格分开。如www.xxx.com www.yyy.com。
        * 多个域名共享同一个ip地址，有效解决ip地址不足的问题
    * 基于端口的虚拟主机
* error_page
    * 错误页面
    ```
    error_page 500 502 404 /50x.html
    ```
    
* resolver 
    * resolver DNS服务器的ip地址
    * resolver_timeout time 设置DNS服务器的超时时间
* set $api_url

* 逻辑控制语句 
    * if
    * 没有else
    * 没有&&
    * 如何实现nginx的多条件判断
    ```
        set $tag 0;
        if ($env = "prod") {
            set $tag 1;
        }

        if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
            set $tag "${tag}1";
        }
        if ($tag = "11") {
            rewrite ^.+ http://xxxxxx  break;
        }

    ```

* server块中常用的匹配
    * ~  区分大小写匹配
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
            * 区分大小写匹配
        * 例如: location = / {}
            * 只匹配/
        * 例如： location / {}
            * 匹配任何以/开头的查询
    * proxy_pass
        * proxy_pass用来设置反向代理
        ```
        server { 
            listen 12800; 
            server_name www.xxx.com; 
            location /aa/ { 
                # 请求localhost:12800/aa/a.html会被代理到http://www.yyy.com/bb/a.html
                proxy_pass http://www.yyy.com/bb/; 
            }

            location /bb/ { 
                proxy_pass http://www.yyy.com$request_uri; 
            }  
        }
        ```
    * root
        * root用来设置静态目录
        * 例如: 
        ```
            # 请求uri地址是/file/aa.png将会返回文件/var/www/www.xxx.com/file/aa.png， location指定的路径file不会被丢弃
            location /file/ {
                root /var/www/www.xxx.com
            }

        ```
    * alias
        * alias和root的作用是一样的，不同的是location指定的file路径会被丢弃
        * 例如
        ```
            # 请求uri地址是/file/aa.png将会返回文件/var/www/www.xxx.com/aa.png， location指定的路径file被丢弃
            location /file/ {
                alias /var/www/www.xxx.com
            }
        ```

    * random_index on|off
        ```
            location / { 
                root   /var/www/www.xxx.com;
                # 随机选择该目录下的页面作为主页
                random_index on;
            } 
        ```
    * rewrite 重写
    * 什么时候使用proxy_pass, 什么时候使用rewrite
        * rewrite只针对除了域名和参数之外的url路径部分起作用，即如果url是http://xxx.yyy.com/aa/bb/cc/index.html?a=1&b=2, <br/>
        rewrite只能针对/aa/bb/cc/index.html重写
        * 如果想对全部的url做替换，需要使用proxy_pass

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
    * server => 配置分流到的服务器， 如果某台服务器down了，能够自动剔除
    * weight => nginx的轮询比率，分流到不同服务器的权重 => weight越大，权重越大
    * down => 指的是当前server不参与负载均衡, 用法： server ip1:12800 down;

* 如何配置upstream？
```
// 每次请求分流到不同的服务器。如果流量过大，其中某个server down了，或者端口被关(可以通过iptables设置)，负载均衡会检测到并分流到其他的server。

http {
    upstream xxxname {
        server ip1:12800 weight=1;
        server ip2:12801 weight=2;
        server ip3:12802 weight=2;
    }

    server {
        listen 80;
        server_name localhost;
        location / {
            proxy_pass http://xxxname;
        }
    }
}

```

* 配置多个upstream负载均衡
```
http {
    upstream name_one {
        server ip1:12800 weight=1;
        server ip2:12801 weight=2;
        server ip3:12802 weight=2;
    }

    upstream name_two {
        server ip1:12803 weight=1;
        server ip2:12804 weight=2;
        server ip3:12805 weight=2;
    }

    server {
        listen 80;
        server_name localhost;
        location / {
            if ($request_uri ~ "^\/one") {
                proxy_pass http://name_one;
            }

            if ($request_uri ~ "^\/two") {
                proxy_pass http://name_two;
            }
        }
    }
}

```

* 负载均衡如何做到某个客户端固定访问一个服务器，解决session一致的问题 => 可以通过ip_hash
```
upstream xxxname {
    ip_hash;
    server ip1:12800;
    server ip2:12801;
    server ip3:12802;
}
```
* fair => 按照响应时间来分配请求， 响应时间短的服务器会优先分配
```
upstream xxxname {
    server ip1:12800;
    server ip2:12801;
    server ip3:12802;
    fair;
}
```

#### http迁移到https
```
    location / {
        return 302 https://$host$request_uri;
    }

```
#### 设置反盗链

```
location ~ .*\.(jpg|jpeg|JPG|png|gif|icon)$ {
        valid_referers blocked www.xxx.com xxx.com;

        // 凡是从www.xxx.com, xxx.com跳过来的请求都会return 404
        if ($invalid_referer) {
            return 404;
        }
}
```

#### server虚拟主机配置proxy_set_header
* proxy_set_header X-Real-IP $remote_addr;
    * 经过nginx反向代理，服务器取到的客户端的ip地址将会是nginx的ip地址, 但是nginx是可以拿到真实的客户端ip的。<br/>
    想要服务器端获取用户真实的ip，只需要在nginx的虚拟主机中设置：
    ```
    server {
        listen       12800;
        server_name  localhost;

        proxy_set_header X-Real-IP $remote_addr;
    }
    ```
* proxy_set_header Host $host;
    * $host即是nginx的ip, 服务器端可以通过log查看到nginx服务器的地址
* proxy_set_header REMOTE-HOST $remote_addr;
    * $remote_addr => 客户端ip

* proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

* proxy_set_header Referer $http_referer;
    * $http_referer => 网页来源
