redis笔记
========
[toc]

# replication
## master-slave
repo-backlog-size 1mb  缓存大小
slave-priority 100
配置为0，永远不会被选为master
## slave-slave

# problem

## 安装
出现问题：jemalloc/jemalloc.h: No such file or directory
使用命令：make MALLOC=libc

## fork fail

> https://access.redhat.com/documentation/zh-TW/Red_Hat_Enterprise_Linux/6/html/Performance_Tuning_Guide/s-memory-captun.html

> 参数：/proc/sys/vm/overcommit_memory   

              0: heuristic overcommit (this is the default)
              1: always overcommit, never check
              2: always check, never overcommit
              In mode 0, calls of mmap(2) with MAP_NORESERVE set are not checked, and the default check is very weak, leading to the risk of getting a process "OOM-killed".  Under Linux 2.4
              any non-zero value implies mode 1.  In mode 2 (available since Linux 2.6), the total virtual address space on the system is limited to (SS + RAM*(r/100)), where SS is the size
              of the swap space, and RAM is the size of the physical memory, and r is the contents of the file /proc/sys/vm/overcommit_ratio.

# 将来计划

future:
     https://github.com/antirez/redis/issues/3097

new psync:
     https://gist.github.com/antirez/ae068f95c0d084891305

# 协议：
## 通信协议
    - For Simple Strings the first byte of the reply is "+"
    - For Errors the first byte of the reply is "-"
    - For Integers the first byte of the reply is ":"
    - For Bulk Strings the first byte of the reply is "$"
    - For Arrays the first byte of the reply is "*"

## RDB

	1. rdb文件格式
	https://github.com/sripathikrishnan/redis-rdb-tools/wiki/Redis-RDB-Dump-File-Format
	1. rdb文件格式历史
	https://github.com/sripathikrishnan/redis-rdb-tools/blob/master/docs/RDB_Version_History.textile
	1. redis作者写的rdb文件格式与其它数据持久化格式之间的异同
	http://oldblog.antirez.com/post/redis-persistence-demystified.html
