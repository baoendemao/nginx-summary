* nginx的upstream模块使用的是一致性哈希算法来分配节点的。nginx通过配置的分配变量名作为hash的输入，来指定负载均衡策略。常用的三种分配策略：
    * 根据客户端ip
    ```
        upstream somestream {
            consistent_hash $remote_addr;
            server 10.xx.xx.1:12800;
            server 10.xx.xx.2:12800;
            server 10.xx.xx.3:12800;
        }
    ```
    * 根据客户端请求的uri
    ```
        upstream somestream {
            consistent_hash $request_uri;
            server 10.xx.xx.1:12800;
            server 10.xx.xx.2:12800;
            server 10.xx.xx.3:12800;
        }
    ```
    * 根据客户端携带的请求参数
    ```
        upstream somestream {
            consistent_hash $args;
            server 10.xx.xx.1:12800;
            server 10.xx.xx.2:12800;
            server 10.xx.xx.3:12800;
        }
    ```
* 一致性哈希原理
    * 一致性哈希将整个哈希值空间组织成了一个虚拟的圆环，假设某哈希函数H的值空间是0到（2的32次方-1），即哈希值是一个32位无符号整数。其中哈希空间按顺时针方向组织，0和（2的32次方-1）会在12点钟方向重合。

