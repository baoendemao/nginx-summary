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
* 逻辑控制语句 if等

* location{} => location块对应一个站点的路由 => 一个站点可以有多个location
    * location匹配规则
        * 例如： location ~ ^/list.html {}
        * 遵从正则匹配
            * 
                ```
                location \ {
                    root xxxx
                }
                ```
            * location = xxx
                * 等于
            * ~
                * 大小写匹配
    * proxy_pass
        * proxy_pass用来设置反向代理
        * 例子：
            * 例子一 proxy_pass到http:
                ```
                server { 
                    listen 12800; 
                    server_name www.xxx.com; 
                    location / { 
                        proxy_pass http://www.yyy.com; 
                    } 
                }
        
                ``` 
            * 例子二 root到目录
                ```
                server { 
                    listen 12800; 
                    server_name www.xxx.com; 
                    location / { 
                        root   /var/www/www.xxx.com;
                        index  index.html index.htm;
                    } 
                }
        
                ```
            * 例子三 设置api代理，防止跨域（也可以通过webpack的devServer配置来设置proxy /api）
                ```
                server { 
                    listen 12800; 
                    server_name www.xxx.com; 
                    set $api_url http://api.xxx.com;
                    location /api/ { 
                        proxy_pass  $api_url;
                    }
                }
        
                ```
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