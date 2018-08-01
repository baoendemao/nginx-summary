* proxy_set_header 允许重新定义请求头host，如下：
```
server {
    listen       9999;
    server_name  localhost;
    
    # 如果想让Host是www.xxx.com，则设置如下：
    proxy_set_header Host 'www.xxx.com';
        
    location / { 
	    proxy_pass http://localhost:10000;
    }
}

# 下面两个server虽然端口号一样，但是配置的server_name不一样。
# 当localhost:9999发起请求，请求Header的host被设置成了www.xxx.com，
# 所以当proxy_pass到10000端口的时候，会根据域名进行匹配，所以匹配到了下面的第一个server
server {
    listen 10000;
    server_name www.xxx.com;

    location / {
        root   /var/xxx/xxx;
        index  index.html index.htm;
    }
}

server {
    listen 10000;
    server_name www.yyy.com;

    location / {
        root   /var/yyy/yyy;
        index  index.html index.htm;
    }
}

```