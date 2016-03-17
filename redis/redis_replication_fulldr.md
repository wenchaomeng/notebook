redis数据复制方案分析
=============
# 数据源
## 假slave
 作为一个假的slave，连接上redis，进行数据同步，并且将同步后的数据传送至目标。有两种方案，一是连接master，二是连接slave。下面将我们的slave成为FS(fake slave)
### master-FS
- slave不能被选为master
- 初次同步问题
    1. rdb数据产生
	   > fork, 但是fork可能会失败  
       > 处理方式：设置操作系统参数/proc/sys/vm/overcommit_memory
    1. rdb数据传输至slave时间如果过长？
    >master写入的请求如何缓存，是否可能会丢失？或者缓存不足是否可能会引发rdb重新产生，传输？  
    > - **redis实现原理**     
    >  redis写入slave的缓存，然后在rdb传送完成后，将缓存数据再发送给slave  
	> - **解决方案**  
    >  缓存大小控制参数（可以动态生效）：  
    >  `client-output-buffer-limit slave <hard limit> <soft limit> <soft seconds>`  
    >  hard limit：缓存绝对大小  
    >  soft limit：连续seconds秒写入limit数据  
    >  在达到以上限制时，master将会关闭slave的网络连接，导致初次同步失败，引发新的数据同步
    >      
    1. others
#### 
### slave-FS
### 通用问题
#### lua脚本问题
# 数据处理
# 数据载入(写入新的集群)
# 双A
