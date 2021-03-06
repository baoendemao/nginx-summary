* location
    * 例子一 proxy_pass到http （这里可以是node启动的服务）, 通过www.xxx.com访问
        ```
        server { 
            listen 12800; 
            server_name www.xxx.com; 
            location / { 
                proxy_pass http://www.yyy.com; 
            } 
        }

        ``` 
    * 例子二 root到目录， 通过www.xxx.com访问
        ```
        server { 
            listen 12800; 
            server_name www.xxx.com; 
            location / { 
                root   /var/www/www.xxx.com;

                # 配置首页文件，按照从左到右的顺序，如果找不到index.html，则寻找index.htm
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
                # 当访问localhost:12800/api/xxx/yyy，将会转发到http://api.xxx.com/xxx/yyy
                # proxy_pass将使用改变后的uri：rewrite规则里将(.*)传给$1，转发时/$1会拼接在http://api.xxx.com的后面
                rewrite ^/api/(.*) /$1 break;  
                proxy_pass  $api_url;
            }
        }

        ```
