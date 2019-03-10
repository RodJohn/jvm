
# CMS

    Concurrent-Mark-Sweep并发标记清除收集器
    
    是HotSpot在JDK1.5推出的第一款真正意义上的并发收集器；
    第一次实现了让垃圾收集线程与用户线程（基本上）同时工作；

# 定位

    低延时收集器
    老年代收集器

# 技术特点

    使用标记清除算法
    使用并发和并行多线程
    

# 优点

    GC停顿时间短

# 缺点

1.产生内存碎片  
2.无法处理“浮动垃圾”  
3.对cpu资源敏感  
4.不适合大内存  
CMS的Initial Marking和Remarking两个STW阶段在Heap区越来越大的情况下需要的时间越长，  
并且由于内存碎片，需要压缩的话也会造成较长停顿时间。

      
    
# 工作流程

图例

![](https://github.com/RodJohn/JVM/blob/master/img/gccms.png)

过程

    初始标记(initial mark)
        仅标记GC Roots能直接关联到的对象
        （需要stop the world）
    并发标记(concurrent mark)
        查找引用链
        （并发执行）
    重新标记(remark)
        修正并发标记期间因用户程序继续运作而导致标记变动的那一部分对象的标记记录
        （并行执行，需要stop the world）
    并发清除(concurrent sweep)
        （并发执行）
        
分析
        
    最耗费时间的标记与清除阶段都使用并发处理（不需要暂停工作）
    只有初始标记和重新标记需要STW，而且重新标记是并行（快速）
    所以整体的回收是低停顿的。
 
 
 
 
