#### 修改本地hosts来测试nginx配置中的域名
在本地测试带域名的Nginx配置文件是否正确，可以通过修改hosts文件，使域名DNS解析到本地的ip。
* windows
    * 在windows中对于域名的测试可以通过修改本地hosts文件
    ```
        // windows的hosts文件的路径
        C:\Windows\System32\drivers\etc
    ```
* linux
    * 在linux中对域名的测试可以通过修改/etc/hosts文件
    ```
    127.0.0.1   localhost localhost.localdomain
    ```
