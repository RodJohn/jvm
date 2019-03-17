# 4 堆中优先在Eden分配

概述

    大多数情况下，对象在新生代Eden去中分配，
    Eden区没有足够空间进行分配时，虚拟机将发起一次Minor GC
    MinorGC将处理Eden和From Survivor的数据    

作用

    Eden区会频繁进行MinorGC
    便于回收大部分的对象

VM参数

    -XX:+UseSerialGC -XX:+PrintGCDetails -verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8
    把JVM设置了不可扩展内存20MB，其中新生代10MB，老年代10MB，而新生代区域的分配比例是8:1:1，
    使用Serial/Serial Old组合收集器

代码

    private static final int _1MB = 1024*1024;
        
        public static void testAllocation(){
            byte[] allocation1,allocation2,allocation3,allocation4;
    
            allocation1 = new byte[2 * _1MB];
            allocation2 = new byte[2 * _1MB];
            allocation3 = new byte[2 * _1MB];
            allocation4 = new byte[4 * _1MB];
    }
       
GC日志    
        
     [GC (Allocation Failure) 
        [DefNew: 6709K->1023K(9216K), 0.0047844 secs] 
        6709K->1334K(19456K), 0.0055483 secs][Times: user=0.01 sys=0.00, real=0.01 secs] 
     [GC (Allocation Failure) 
        [DefNew: 7323K->0K(9216K), 0.0043779 secs] 
        7633K->7429K(19456K), 0.0044012 secs] [Times: user=0.00 sys=0.00, real=0.01 secs] 
        
     Heap
      def new generation   total 9216K, used 4234K [0x00000000fec00000, 0x00000000ff600000, 0x00000000ff600000)
       eden space 8192K,  51% used [0x00000000fec00000, 0x00000000ff022ad8, 0x00000000ff400000)
       from space 1024K,   0% used [0x00000000ff400000, 0x00000000ff400000, 0x00000000ff500000)
       to   space 1024K,   0% used [0x00000000ff500000, 0x00000000ff500000, 0x00000000ff600000)
      tenured generation   total 10240K, used 7429K [0x00000000ff600000, 0x0000000100000000, 0x0000000100000000)
        the space 10240K,  72% used [0x00000000ff600000, 0x00000000ffd414f0, 0x00000000ffd41600, 0x0000000100000000)
      Metaspace       used 5227K, capacity 5424K, committed 5632K, reserved 1056768K
       class space    used 609K, capacity 659K, committed 768K, reserved 1048576K
    
解析
    
    有“[GC……]”字眼的就是GC日志记录，
        其中GC代表MinorGC，如果是Full GC的话会直接写着Full GC
        Allocation Failure 表示内存分配失败
    Heap以下的信息是JVM关闭前的堆使用情况信息描述。    
    [DefNew: 6824K->268K(9216K), 0.0087400 secs]中
        DefNew代表使用的是代表Serial收集器，
        6824K->268K(9216K)中6284K代表新生代收集前使用内存，268K代表收集后的使用内存，而9216代表新生代的总分配内存，
        0.0087400 secs为新生代收集时间。
    外层的6824K->6412K(19456K)代表整个Java堆的收集前使用内存->收集后使用内存（总分配内存），0.0087927 secs为整个GC的收集时间
       
    allocation1、allocation2、allocation3一共需要6MB，而Eden一共有8MB，优先分配到Eden。
    但再分配allocation4的时候Eden空间不够，执行了一次Minor GC，
    由于Survivor只有1MB，不够存放allocation1、allocation2、allocation3，所以直接迁移到老年代了，
    最后Eden空闲出来了就可以放allocation4了。
    
    最后通过Heap打印信息可以看到JVM内存分配的最后状态，
    “def new generation   total 9216K, used 4446K”为allocation4最后分配新生代所占用的4MB，
    而“tenured generation   total 10240K, used 6144K”则是老年代被allocation1、allocation2、allocation3所占用的6MB了。
    
 
