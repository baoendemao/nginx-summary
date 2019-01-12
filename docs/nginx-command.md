#### nginx -h  查看帮助信息
#### nginx -v  查看版本信息
#### nginx -t    
* 检测配置文件是否正确
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

#### nginx -s 信号量
* nginx -s reload -c /etc/nginx/nginx.conf  
    * 检测nginx的配置文件是否有问题

* nginx -s reload     重新加载配置文件，在不停止nginx进程的前提下，平滑的reload

* nginx -s stop     停止
