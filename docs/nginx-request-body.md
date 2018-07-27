当以一条请求以post的形式发送到nginx，可以通过变量$request_body获取请求的body体。<br/>
但是发现获取到的$request_body变量每次都是空的，查资料了解到nginx对$request_body的获取是有要求的。<br/>

* 
```
    # 官方说明：
    The variable’s value is made available in locations processed by the proxy_pass, fastcgi_pass, uwsgi_pass, and scgi_pass directives when the request body was read to a memory buffer.
```

```
    # 在http块中设置：
    client_max_body_size 100M;
    fastcgi_buffers 32 8k;
    client_body_buffer_size 1024K;

```

* location中不能使用return，会导致获取不到body
```
 location / {
        echo_read_request_body;
        access_log /var/log/nginx/access.log logstash_json;
        default_type application/json;

        # 导致获取不到body
        # return 200 '{"code":"200"}';
    }

```