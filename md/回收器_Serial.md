
   
# 定位

    新生代收集器
    客户端收集器
    
    
    

# 优点

    使用复制算法
    单cpu下效率高
    
# 缺点

    串行收集，用户线程必须暂停

# 工作流程

![](https://github.com/RodJohn/JVM/blob/master/img/gcserial.png)

    
# 适用情况    
    
    client模式下（内存小，回收快）
    单核CPU情况下（无需线程切换）
    
 
 
# Serial Old

    Serial Old回收器是Serial的老年代版本
    使用标记整理算法
    工作模式、技术特点、使用情况同于Serial回收器   
    
    另外在Server模式下，
    作为CMS收集器的后备预案，在并发收集发生Concurrent Mode Failure的时候使用。
  
 
 
