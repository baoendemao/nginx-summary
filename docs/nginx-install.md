#### 通过yum安装和卸载nginx
```
yum install nginx
yum list | grep nginx
yum -y remove 'nginx'

```

### 通过源码安装nginx
```
// 下载nginx源码包
wget 'http://nginx.org/download/nginx-1.11.2.tar.gz'

// 解压nginx源码包
 tar -zvxf nginx-1.11.2.tar.gz

// 下载echo模块：echo-nginx-module-0.61.tar.gz

// 解压echo模块

// 配置： --prefix指的是安装到/etc/nginx路径， --add-module指的是解压后的echo模块目录
 ./configure --prefix=/etc/nginx  --add-module=/usr/nginx/echo-nginx-module-0.61

make -j2

make  install

```
* 源码安装时报错:  configure: error: the HTTP rewrite module requires the PCRE library.
    * 是因为缺少pcre-devel库，安装： yum -y install pcre-devel
