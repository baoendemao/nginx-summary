## master和worker进程模型
### nginx是多进程的，而不是多线程的
* nginx为什么不采用多线程的模型 
    * （1）对于线程而言，是共享同一块地址空间的，如果一旦有一个第三方模块发生错误，会造成地址空间错误，会引发nginx进程挂掉
    * （2）nginx被设计为高并发，高可用性，服务于unix服务器中。而企业的unix服务器多为多核cpu，为了充分利用多核cpu，且为了避免进程间切换造成的性能消耗，多个worker进程通常被分配在不同的cpu上，且worker进程的数目通常被设为接近unix服务器的cpu的核数
* nginx的master-worker进程模型是如何工作的  
    * 例如下图所示：
    ![image](https://github.com/baoendemao/front-end-engineering/blob/master/nginx-summary/images/master-worker-command.png)
