

# 5 Java堆

概述

    Java堆（Java Heap）
    堆

作用

    用来存储对象以及数组
    但是并不是全部的实例对象都在堆中（栈上分配）
    
划分

    在采用分代收集算法的JVM中
    hotspot中    
    堆由新生代和老年代组成，
    新生代由Eden区，From Survivor区，To Survivor区组成
    默认情况下各自占比 8:1:1        
 
特点

    堆是线程共享
    但是对于具体的ThreadLocal数据区是线程私有的
    
    
    
# 线程本地缓存

    堆是所有线程共享的，因此在堆上分配内存需要加锁，
    Sun JDK为提升效率，会为每个新建的线程在Eden上分配一块独立的空间由该线程独享，
    这块空间称为TLAB（Thread Local Allocation Buffer）
    。其大小由JVM根据运行情况计算得到，也可通过参数-XX:TLABWasteTargetPercent来设置TLAB可占用的Eden空间的百分比，默认值为1%。
    在TLAB上分配内存不需要加锁，因此JVM在给线程中的对象分配内存时会尽量在TLAB上分配。
    如果对象过大或TLAB用完，则仍然在堆上进行分配。    
    
   