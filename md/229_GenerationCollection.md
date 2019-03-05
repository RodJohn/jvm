# 分代收集算法

    根据对象存活的特点划分区域
    不同区域用不同回收算法回收垃圾，提高GC效率
    是目前大部分JVM的垃圾收集器采用的算法
    

# 区域划分


![](https://github.com/RodJohn/JVM/blob/master/img/gc%E5%88%86%E4%BB%A3%E7%A9%BA%E9%97%B4.png)


# 新生代

算法

    对象存活时间短，一般采取Copying算法    

过程

    GC开始时，对象只会存在于Eden区和From Survivor区，To Survivor区是空的（作为保留区域）。
    GC进行时，Eden区中所有存活的对象都会被复制到To Survivor区，
    而在From Survivor区中，
    仍存活的对象会根据它们的年龄值和总量决定去向，被移到老年代中或者To Survivor区。
    接着清空Eden区和From Survivor区，
    接着， From Survivor区和To Survivor区会交换它们的角色，
    （也就是新的To Survivor区就是上次GC清空的From Survivor）
    GC时当To Survivor区没有足够的空间存放上一次新生代收集下来的存活对象时，需要依赖老年代进行分配担保，将这些对象存放在老年代中。

特点

    新生代中Eden区偏大的意义
       对象存活时间短，提高空间利用率
    survivor区存在两个的意义
        通过对象在Surviver区间的移动（也就是年龄）来控制对象进入年老区的时机，提高GC效率
        （新生代GC也叫做minorGC）



# 年老带

特点

    对象存活时间长
    采用Mark-Compact、MarkSweep 算法

过程

    如果年老带内存不够，就会发起fullGc，对整个堆空间进行回收，会引起长时间的STW

# 永久代

    永久代主要是回收废弃常量和无用的类


