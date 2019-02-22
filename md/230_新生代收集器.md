

# Serial

![](https://github.com/RodJohn/JVM/blob/master/img/gcserial.png)
    
作用

    新生代收集器
    
特点

    使用复制算法
    使用单线程进行回收
    回收时暂停其他所有线程
    
适用情况    
    
    client模式下（内存小，回收快）
    单核CPU情况下（无需线程切换）
    
    
# ParNew
![](https://github.com/RodJohn/JVM/blob/master/img/gcparnew.png)

作用

    新生代收集器
    
特点

    Serial的多线程版本    
        使用多线程进行回收
        回收时暂停其他所有线程    
        
适用情况    
    
    多核CPU情况下
       
    ParNew收集器在单CPU的环境中绝对不会有比Serial收集器更好的效果，
    甚至由于存在线程交互的开销，该收集器在通过超线程技术实现的两个CPU的环境中都不能百分之百的保证能超越Serial收集器。
    当然，随着可以使用的CPU的数量增加，它对于GC时系统资源的利用还是很有好处的。
    它默认开启的收集线程数与CPU的数量相同，
    可以使用-XX:ParallelGCThreads参数来限制垃圾收集的线程数。
    
    除Serial外，只有它能与CMS收集器配合工作
    
    
# Parallel Scavenge

作用 
    
    新生代收集器
    
特点

    使用复制算法   
    使用多线程
    关注吞吐量
        （吞吐量=运行用户代码时间/CPU总时间)


参数        
    
    -XX:MaxGCPauseMillis，最大垃圾回收停顿时间。
    这个参数的原理是空间换时间，收集器会控制新生代的区域大小，从而尽可能保证回收少于这个最大停顿时间。简单的说就是回收的区域越小，那么耗费的时间也越小。
    所以这个参数并不是设置得越小越好。设太小的话，新生代空间会太小，从而更频繁的触发GC。
    -XX:GCTimeRatio，垃圾回收时间与总时间占比。这个是吞吐量的倒数，原理和MaxGCPauseMillis相同。
    
    因为Parallel Scavenge收集器关注的是吞吐量，所以当设置好以上参数的时候，同时不想设置各个区域大小（新生代，老年代等）。
    可以开启**-XX:UseAdaptiveSizePolicy**参数，让JVM监控收集的性能，动态调整这些区域大小参数。
    
        
适用情况
    
    停顿时间越短就越适合需要与用户交互的程序，良好的响应速度能够提升用户的体验；
    而高吞吐量则可以最高效率地利用CPU时间，尽快地完成程序的运算任务，主要适合在后台运算而不需要太多交互的任务。
 
