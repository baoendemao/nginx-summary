* nginx可以作为web server接收来自前端的ajax请求，配置如下：
```

# 允许跨域
add_header         'Access-Control-Allow-Origin' '*';
# 允许ajax请求携带cookie
add_header         "Access-Control-Allow-Credentials" "true";add_header         "Access-Control-Allow-Headers" "x-requested-with,content-type";
proxy_set_header   Host             $host;
proxy_set_header   X-Real-IP        $remote_addr;
proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;

location /test.html{ 
    default_type application/json;
    return 200 '{"code":"0"}';
}

```

<br/>

ajax请求代码如下：
```
var ajax = new XMLHttpRequest();
ajax.open('post','test.html');
ajax.send();
ajax.onreadystatechange = function () {
    console.log(ajax.status);        // 打印 nginx配置的返回状态码200
    
    console.log(ajax.responseText);  // 打印 nginx配置的code: {"code":"0"}
}

```