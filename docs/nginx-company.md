## nginx在企业中的应用
### 负载均衡，作为upstream server
* (1) nginx通常被放在企业内网的边缘节点上，当用户请求到来时，通过nginx负载均衡，将请求分发到不同的服务器上，充当分流的作用。
* (2) 对于流量比较大的企业，会购买硬件负载均衡设备，即硬件负载均衡来分流量，nginx来分业务，各自负责不同的职责。硬件负载均衡将流量分发到多个nginx server上，然后根据请求的域名和端口号来分发业务，这时请求到达具体的工程server中，根据location来找到具体的路由。
### static server
* （1）nginx作为静态server是最简单的一种配置方法，只需要准备好打包好的html, js, css，就可以通过ip + 端口号访问对应的location页面。
* （2）在一台linux服务器中，可以根据域名的不同或者端口号的不同，配置多个不同的nginx静态server，此时通常根据端口号的不同配置成不同的conf文件，然后在nginx.conf中include，当nginx reload的时候，worker进程会重新监听并分发请求到不同的nginx静态server中。
### 路由proxy pass
* 路由主要是基于nginx的location，可以根据请求参数的不同，proxy pass到另一个server中，比如node起的另一个server。
### api反向代理
* 现在前后端分离，前端和后端分别负责管理自己的服务器。对于api的请求，通常会存在浏览器跨域的问题。所以在前端的nginx server上将api反向代理，可以绕过跨域的问题。
### access log
* 对于一条请求先到达nginx server，在access log中可以分析这条请求的详情，比如ip，请求时间，请求method, 请求url。
### error log
* 当nginx处理请求异常的时候，在error log中可以查看相应的错误信息。
### 动态缓存
* 将一些不怎么改变的内容可以缓存在nginx层，加速对用户的访问。

## tengine
* 官网： http://tengine.taobao.org/book/index.html
* (1) tengine在nginx的基础上，针对大访问量网站的需求，添加了很多高级功能和特性。
* (2) tengine的性能和稳定性已经在大型的网站如淘宝，天猫等得到了很好的检验，它的最终目标是打造一个高效、稳定、安全、易用的web平台。tengine在阿里的生态下，得到了很多考验，修改了nginx的许多主干代码，但是无法跟着nginx的官方版本做同步的升级，
* (3) 虽然tengine可以使用nginx的第三方模块，但是tengine无法跟着nginx的官方版本做同步的升级，所以有些网友不太推荐使用tengine