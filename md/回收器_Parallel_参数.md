# 并行线程数

    -XX:ParallelGCThreads=n:
    设置并行收集器收集时使用的CPU数
    一般设置为最大CPU数



# 最大垃圾回收停顿时间
    
参数
    
    -XX:MaxGCPauseMillis

原理
    
    这个参数的原理是空间换时间，
    收集器会控制新生代的区域大小，从而尽可能保证回收少于这个最大停顿时间。
    所以这个参数并不是设置得越小越好。设太小的话，新生代空间会太小，从而更频繁的触发GC。
    
GCTimeRatio
    
    -XX:GCTimeRatio，垃圾回收时间与总时间占比。这个是吞吐量的倒数，原理和MaxGCPauseMillis相同。

# 自适应调节策略
    
参数

    -XX:UseAdaptiveSizePolic
    
过程
    
    Parallel Scavenge收集器能够配合自适应调节策略，把内存管理的调优任务交给虚拟机去完成。
    只需要把基本的内存数据设置好（如-Xmx设置最大堆），
    然后使用MaxGCPauseMillis参数（更关注最大停顿时间）或GCTimeRatio参数（更关注吞吐量）给虚拟机设立一个优化目标，
    让JVM监控收集的性能，动态调整这些区域大小参数
 
 
 
