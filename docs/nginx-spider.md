* nginx可以通过爬虫的userAgent来禁止某个爬虫的UA
```
    location / {
        if ($http_user_agent ~* "xxspider") {

            return 403;

        }
    }
```

* 常见的爬虫的userAgent：qihoobot|Baiduspider|Googlebot|Googlebot-Mobile|Googlebot-Image|Mediapartners-Google|Adsbot-Google|Feedfetcher-Google|Yahoo! Slurp|Yahoo! Slurp China|YoudaoBot|Sosospider|Sogou spider|Sogou web spider|MSNBot|ia_archiver|Tomato Bot

* 网站上可以添加robots.txt文件来配置
    * 将该文件存放在站点的根目录下，它会告诉蜘蛛程序在服务器上什么文件是可以被爬取的
    * 当蜘蛛爬虫访问一个站点的时候，首先会检查站点下是否存在该文件
        * 存在：按照robots.txt的配置来访问, 可以设置禁止爬取的页面
        * 不存在：根据链接抓取
    * 配置例子
        * 禁止xxspider访问网站
        ```
            User-agent: xxspider
            Disallow: /
        ```
        * 禁止xxspider抓取图片
        ```
            User-agent: xxspider
            Disallow: /*.gif$
        ```