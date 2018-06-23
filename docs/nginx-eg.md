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