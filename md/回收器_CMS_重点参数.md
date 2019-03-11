

回收优化


# 空间碎片

## 原因

    标记清除算法的影响
    

## 解决
    
碎片整理
    
    GC时之后整理内存碎片（标记整理了）
    整理碎片需要STW
    碎片问题解决了,但停顿时间又变长了。
    
碎片整理频率
    
    可以设置执行多少次不压缩的Full GC后，跟着来一次带压缩的
    
## 参数
    
    UseCMSCompactAtFullCollection 开启碎片整理
        (默认开启，但不会进行，结合下面的CMSFullGCsBeforeCompaction)，
    CMSFullGCsBeforeCompaction  设置整理频率
        (默认为0，每次进入Full GC时都进行碎片整理）。

    

# 浮动垃圾

## 原因

    在并发清除时，用户线程新产生的垃圾，称为浮动垃圾
    这使得并发清除时需要预留一定的内存空间，
    否则线程申请不到足够空间，就会产生Concurrent  Mode Failure错误，
    进而jvm将使用SerialOld回收器进行FullGC
    
    
## 解决

    设置回收阈值
    达到百分比就进行垃圾回收。
    导致内存使用率下降

## 参数值
   
默认参数   
       
    JDK1.5默认值为68%；
    JDK1.6变为大约92%；        
      
在实际情况中

    CMS是配合ParNew进行使用，
    此时新生代大小一般是堆的60%
    加长分代年龄减少对象进入老年代的速率        
    该值设置为80%
 
 
## 参数

    -XX:CMSInitiatingOccupancyFraction  设置回收阈值
	-XX:+UseCMSInitiatingOccupancyOnly  设置阈值不变化
		如果不指定,JVM仅在第一次使用设定值,后续则自动调整
   
   
# 抢占CPU资源
    
## 原因

    CMS默认的回收线程数是(CPU个数+3)/4。
    当CPU大于4个时,保证回收线程占用至少25%的CPU资源
    当CPU只有2个时,保证回收线程占用50%的CPU资源
    会造成用户线程执行效率下降

## 解决

    CMS的解决方案是提供了一个 incremental mode（增量模式）。
    在这种模式下，进行并发标记、清理时让GC线程、用户线程交替运行，尽量减少GC线程独占CPU资源的时间。
    这会造成GC时间更长，但对用户线程造成的影响就会少一些。
    但实践证明，这种模式下CMS的表现很一般，并没有什么大的优化。
    但效果并不理想，JDK1.6后就官方不再提倡用户使用。



    





