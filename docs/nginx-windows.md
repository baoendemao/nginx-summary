* 对于windows的用户，可以将nginx.exe安装为系统服务运行，以便于每次开机自启动。安装方法：
    * 下载"Windows Service Wrapper"，放在nginx安装目录中，并重命名为myapp.exe。
    * 新建myapp.xml
    ```
    <service>    
        <id>nginx</id>    
        <name>nginx</name>    
        <description>nginx</description>    
        <executable>D:/nginx/nginx.exe</executable>    
        <logpath>D:/nginx/logs</logpath>    
        <logmode>roll</logmode>    
        <depend></depend>    
        <startargument>-p D:/nginx</startargument>    
        <stopargument>-p D:/nginx -s stop</stopargument>    
    </service>    
    ```
    * 执行nginx.exe install, 在系统服务中看到nginx服务就说明配置成功
    * 之后可以在cmd中使用net start/stop nginx来启动关闭nginx