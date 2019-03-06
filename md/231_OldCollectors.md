
 # Serial Old 
 
作用
 
    老年代收集器
    
特点

    使用标记整理算法
    使用单线程 
    收集时暂停所有用户进程
    
    
适用情况

    Client模式下的虚拟机使用。
    在Server模式下，作为CMS收集器的后备预案，在并发收集发生Concurrent Mode Failure的时候使用。
    
    
# Parallel Old


作用

    老年代收集器
    
特点

    
    采用多线程
    采用”标记－整理”算法
    关注吞吐量
    
适用情况
    
    Parallel Old收集器是Parallel Scavenge收集器的老年代版本，
    在注重吞吐量及CPU资源敏感的场合，都可以优先考虑Parallel Scavenge加Parallel Old收集器。   
    
    
# CMS

![](https://github.com/RodJohn/JVM/blob/master/img/gccms.png)

## 作用

    老年代收集器


## 特点

    使用标记清除算法
    使用多线程
    GC停顿时间短
    JDK1.5中发布的CMS收集器
    
## 工作流程

    初始标记(initial mark)：找GC Roots （stop the world）
    并发标记(concurrent mark)：并发找引用链
    重新标记(remark)：查找变更     （stop the world）
    并发清除(concurrent sweep)：并发清除
    由于最耗费时间的并发标记与并发清除阶段都不需要暂停工作，所以整体的回收是低停顿的。
 
 
## 缺点


空间碎片

    基于标记清除算法实现的，会导致有大量的空间碎片产生，
    
    在为大对象分配内存的时候，往往会出现老年代还有很大的空间剩余，但是无法找到足够大的连续空间来分配当前对象，不得不提前开启一次Full GC。
    
    为了解决这个问题，CMS收集器默认提供了一个-XX:+UseCMSCompactAtFullCollection收集开关参数（默认就是开启的)，
    用于在CMS收集器进行FullGC完开启内存碎片的合并整理过程，内存整理的过程是无法并发的，这样内存碎片问题倒是没有了，不过停顿时间不得不变长。
    虚拟机设计者还提供了另外一个参数-XX:CMSFullGCsBeforeCompaction参数用于设置执行多少次不压缩的FULL GC后跟着来一次带压缩的
    （默认值为0，表示每次进入Full GC时都进行碎片整理）。     
        
对cpu资源敏感

     垃圾收集线程数和每个收集线程占用cpu的时间受cpu数量影响
    
无法处理“浮动垃圾”

     即并发清除阶段用户线程又产生的对象。
     -XX:CMSInitiatingOccupancyFranction：老年代被使用的百分比，达到时触发GC（如果等老年代占满了再GC，则GC时并发产生的对象可能就获取不到存储空间）
     CMSInitiatingOccupancyFranction过高会导致大量Concurrent Mode Failure，即老年代预留的内存无法满足程序需要。
        


 
