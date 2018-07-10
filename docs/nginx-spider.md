* robots.txt
    * 将该文件存放在站点的根目录下
    * 当蜘蛛爬虫访问一个站点的时候，首先会检查站点下是否存在该文件
        * 存在：按照robots.txt的配置来访问
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