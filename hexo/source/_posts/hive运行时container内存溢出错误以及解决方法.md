---
title: hive运行时container内存溢出错误以及解决方法
date: 2017-01-29 15:40:25
tags: Hive
categories: Hive
---

# 错误提示代码

> Container killed on request. Exit code is 143

## 参考博客

[点我](http://blog.csdn.net/yijichangkong/article/details/51332432)

# 错误描述

SQL三张表做join运行出错；

用hive执行报错如下：

> Diagnostic Messages for this Task: Container [pid=27756,containerID=container_1460459369308_5864_01_000570] is running beyond physical memory limits. Current usage: 4.2 GB of 4 GB physical memory used; 5.0 GB of 16.8 GB virtual memory used. Killing container. 
Container killed on request. Exit code is 143 
Container exited with a non-zero exit code 143

用spark运行报错如下：

> Error: org.apache.spark.SparkException: Job aborted due to stage failure: Task 369 in stage 1353.0 failed 4 times, most recent failure: Lost task 369.3 in stage 1353.0 (TID 212351, cnsz033139.app.paic.com.cn): ExecutorLostFailure (executor 689 exited caused by one of the running tasks) Reason: Container marked as failed: container_1460459369308_2154_01_000906 on host: cnsz033139.app.paic.com.cn. Exit status: 143. Diagnostics: Container killed on request. Exit code is 143 
Container exited with a non-zero exit code 143 
Killed by external signal

# 错误分析

从hive报错看是由于物理内存达到限制，导致container被kill掉报错。 
从报错时刻看是执行reduce阶段报错；故可能reduce处理阶段container的内存不够导致。

# 解决方案
首先查看关于container内存的配置：
> hive (default)> SET mapreduce.map.memory.mb;mapreduce.map.memory.mb=4096hive (default)> SET mapreduce.reduce.memory.mb;mapreduce.reduce.memory.mb=4096hive (default)> SET yarn.nodemanager.vmem-pmem-ratio;yarn.nodemanager.vmem-pmem-ratio=4.2

因此，单个map和reduce分配物理内存4G；虚拟内存限制4*4.2=16.8G；

单个reduce处理数据量超过内存4G的限制导致；设置 mapreduce.reduce.memory.mb=8192 解决；
参考：
[http://stackoverflow.com/questions/29001702/why-yarn-java-heap-space-memory-error?answertab=oldest#tab-top](http://stackoverflow.com/questions/29001702/why-yarn-java-heap-space-memory-error?answertab=oldest#tab-top)

> There are memory settings that can be set at the Yarn container level and also at the mapper and reducer level. Memory is requested in increments of the Yarn container size. Mapper and reducer tasks run inside a container.
mapreduce.map.memory.mb and mapreduce.reduce.memory.mb
above parameters describe upper memory limit for the map-reduce task and if memory subscribed by this task exceeds this limit, the corresponding container will be killed.
These parameters determine the maximum amount of memory that can be assigned to mapper and reduce tasks respectively. Let us look at an example: Mapper is bound by an upper limit for memory which is defined in the configuration parameter mapreduce.map.memory.mb.
However, if the value for yarn.scheduler.minimum-allocation-mb is greater than this value of mapreduce.map.memory.mb, then the yarn.scheduler.minimum-allocation-mb is respected and the containers of that size are given out.
This parameter needs to be set carefully and if not set properly, this could lead to bad performance or OutOfMemory errors.
mapreduce.reduce.java.opts and mapreduce.map.java.opts
This property value needs to be less than the upper bound for map/reduce task as defined in mapreduce.map.memory.mb/mapreduce.reduce.memory.mb, as it should fit within the memory allocation for the map/reduce task.
