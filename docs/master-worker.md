## master和worker进程模型
### nginx是多进程的，而不是多线程的
* nginx为什么不采用多线程的模型 
    * （1）对于线程而言，是共享同一块地址空间的，如果一旦有一个第三方模块发生错误，会造成地址空间错误，会引发nginx进程挂掉
    * （2）nginx被设计为高并发，高可用性，服务于unix服务器中。而企业的unix服务器多为多核cpu，为了充分利用多核cpu，且为了避免进程间切换造成的性能消耗，多个worker进程通常被分配在不同的cpu上，且worker进程的数目通常被设为接近unix服务器的cpu的核数
* nginx的master-worker进程模型是如何工作的  
    * 例如下图所示：
    ![avatar](/images/master-worker-command.png)    <br/>
    可以看出每次reload, master进程id不会改变，会重新分配两个新的worker进程id
    * master进程的职责
        * master进程管理worker进程
    * worker进程的职责
        * worker进程来处理请求
* master进程是通过信号管理worker进程的
    * master可以接收的信号如： TERM, INT, QUIT, HUP, USR1, USR2, WINCH
* worker进程之间共享数据是通过共享内存的方式
    * 多个worker进程之间共享内存的时候，需要加锁

### cpu亲和
* cpu亲和：把cpu核心和nginx worker进程绑定的方式，把每个worker进程固定在一个cpu上执行，减少cpu切换带来的性能损耗，获取更好的性能。
* 可以配置worker_cpu_affinity将worker进程和cpu核心绑定在一起, 即配置表示每个worker进程使用的是哪个cpu核心

### nginx如何平滑的reload
* (1) 向master进程发送HUP信号，即reload命令 
* (2) master进程校验配置语法是否正确，即nginx -t
* (3) master进程打开新的监听端口
* (4) master进程用新的配置启动新的worker进程
* (5) master进程向老的worker进程发送QUIT信号
* (6) 老worker进程关闭监听句柄，老的worker进程不去监听新的请求了， 之后结束进程，