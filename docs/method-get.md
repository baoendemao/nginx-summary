* 配置只允许get和post方法

```
server {
    listen       10000;
    server_name  localhost;

    location / {
        if ($request_method !~ ^(GET|POST)$) {
            return 403;
        }
        proxy_pass  http://localhost:xxxx;
    }
}

```