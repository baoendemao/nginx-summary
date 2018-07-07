* nginx可以作为web server接收来自前端的ajax请求，配置如下：
```
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