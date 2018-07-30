# nginx-summary 
* nginx
    * http反向代理服务器
    * 静态资源web服务
    * 负载均衡调度器SLB
    * 动态缓存
    * nginx的优点
        * IO多路复用epoll
        * 代码模块化
* nginx相关文件
    * /etc/nginx  => 配置文件目录
    * /etc/nginx/nginx.conf   => 总的配置文件
    * /var/log/nginx   => nginx log
    * /usr/sbin/nginx   => which nginx
* 系统安装nginx
    * [安装和卸载nginx](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-install.md)
    * [在windows中如何将nginx安装为系统服务](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-windows.md)

* 配置和使用
    * [nginx相关命令](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-command.md)
    * [nginx conf配置文件详解](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-conf.md)
    * [nginx的log配置](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-log-format.md)
    * [nginx配置location示例](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-eg.md)
    * [针对爬虫的配置](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-spider.md)
    * [在windows中本地测试nginx域名配置](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-test.md)
    * [nginx如何作为server接收ajax请求](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-server.md)
    * [配置proxy_set_header的host来匹配server](https://github.com/baoendemao/nginx-summary/tree/master/docs/proxy-header-host.md)


* 原理相关
    * [一致性哈希算法](https://github.com/baoendemao/nginx-summary/tree/master/docs/consistent_hash.md)
    * [正向代理和反向代理](https://github.com/baoendemao/nginx-summary/tree/master/docs/agent.md)

* 其他
    * [常见问题解决](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-issue.md)
    * [获取不到$request_body](https://github.com/baoendemao/nginx-summary/tree/master/docs/nginx-request-body.md)
