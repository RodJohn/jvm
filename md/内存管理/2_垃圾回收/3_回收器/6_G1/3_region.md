
region

    在G1中，堆被划分成 许多个连续的区域(region)。
    每个区域大小相等，在1M~32M之间。JVM最多支持2000个区域，可推算G1能支持的最大内存为2000*32M=62.5G。
    区域(region)的大小在JVM初始化的时候决定，也可以用-XX:G1HeapReginSize设置。

好处

    避免长暂停时间，可以考虑将堆分成多个部分，一次收集其中一部分，
    这样的方式又叫做增量收集(incremental collection), 分代收集也可以看成一种特殊的增量收集。
    


分代

    传统的GC收集器将连续的内存空间划分为新生代、老年代和永久代（JDK 8去除了永久代，引入了元空间Metaspace），
    这种划分的特点是各代的存储地址（逻辑地址，下同）是连续的

    在G1中没有物理上的Yong(Eden/Survivor)/Old Generation，它们是逻辑的，使用一些非连续的区域(Region)组成的。
    
    这样的分区可以有效避免内存碎片化问题。


Humongous regions

    一种类型的region，叫做Humongous regions，用来存储超过一个region50%的大对象，实际上是连续的若干个region的组合。目前G1对大对象的优化比较弱，因此建议避免使用大对象。

    在上图中，我们注意到还有一些Region标明了H，它代表Humongous，这表示这些Region存储的是巨大对象（humongous object，H-obj），即大小大于等于region一半的对象。H-obj有如下几个特征： * H-obj直接分配到了old gen，防止了反复拷贝移动。 * H-obj在global concurrent marking阶段的cleanup 和 full GC阶段回收。 * 在分配H-obj之前先检查是否超过 initiating heap occupancy percent和the marking threshold, 如果超过的话，就启动global concurrent marking，为的是提早回收，防止 evacuation failures 和 full GC。



