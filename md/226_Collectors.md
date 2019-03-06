# 垃圾收集器


    现代的商用虚拟机的都是采用分代收集的，不同的区域用不同的收集器
    
    Serial、ParNew、Parallel Scavenge用于新生代；
    CMS、Serial Old、Paralled Old用于老年代。
    并且他们相互之间以相对固定的组合使用（具体组合关系如上图）。
    G1是一个独立的收集器不依赖其他6种收集器。ZGC是目前JDK 11的实验收集器。


![](https://github.com/RodJohn/JVM/blob/master/img/gccollectors.png)
   
    垃圾收集中的并行与并发：
    
    并行（Parallel）：多条垃圾收集线程
    并发（Concurrent）：用户线程与垃圾收集线程同时执行




