
# Mixed GC

    Mixed GC
    也叫混合回收
    
    
    
    
# 特点

    回收范围
    可控    
    
    

# 回收范围

    回收所有的年轻代的Region+部分老年代的Region。大对象区域
    
    回收部分老年代是参数-XX:MaxGCPauseMillis，用来指定一个G1收集过程目标停顿时间，默认值200ms，当然这只是一个期望值。
    G1的强大之处在于他有一个停顿预测模型（Pause Prediction Model），
    他会有选择的挑选部分Region，去尽量满足停顿时间，


# 触发时机

    老年代超过阈值
    阈值参数 XX:InitiatingHeapOccupancyPercent，默认值是45%
  
  
    



# 流程

## 来源

![oracle官网](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/index.html)  
两处阶段对不上


## 初始标记

    Root Region Scanning，
    
    (Stop the World Event)
    
    扫描Survivor区中针对老年代的引用
    对Survivor区进行标记(作为根引用区，因为该区内可能有引用指向老年代)。
    
    第一阶段initial mark是共用了Young GC的暂停，这是因为他们可以复用root scan操作，
        会在应用程序运行时发生 并发执行
        这一过程必须在young GC之前完成。
    所以可以说global concurrent marking是伴随Young GC而发生的。
    
    
    
## Concurrent Marking，

    并发标记（Concurrent Marking）。
    
    从GC Root开始对heap中的对象标记，
    标记线程与应用程序线程并行执行，
    并且收集各个Region的存活对象信息。
    
    过程中还会扫描上文中提到的SATB write barrier所记录下的引用。
    在整个堆中进行并发标记(和应用程序并发执行)，此过程可能被young GC中断
    在并发标记阶段，若发现区域对象中的所有对象都是垃圾，那个这个区域会被立即回收(图中打X)

## Remark, 

    再标记，会有短暂停顿(STW)。
    
    标记堆中存活的对象
    再标记阶段是用来收集 并发标记阶段 产生新的垃圾(并发阶段和应用程序一同运行)；
    G1中采用了比CMS更快的初始快照算法:snapshot-at-the-beginning (SATB)。     
     
## Clean up，

    会有STW。
    
    确定存活对象及完全空闲的区域(Stop-The-World)；
    清除Remembered Sets(Stop-The-World)；
    重置空闲区域，并归还给空闲列表(并发)。


## copying

    (Stop-The-World)	
    
    把一部分region里的活对象拷贝到空region里去（并行拷贝），
    只选择收益高的少量region来evacuate，
    这种暂停的开销就可以（在一定范围内）可控。


    该阶段可以在年轻代区域完成(日志记录为[GC pause (young)])，
    也可以在混合的老年代区域和年轻代区域(日志记录为GC Pause (mixed))。
        
OldRegion的选择标准
     
 