

    Young GC，关注于所有年轻代的Region，通过控制收集年轻代的Region个数，从而控制GC的回收时间。
    Mixed GC，关注于所有年轻代的Region，并且加上通过预测计算最大收益的若干个老年代Region。
  

# 新生代收集
Young GC：

选定所有年轻代里的Region。通过控制年轻代的region个数，即年轻代内存大小，来控制young GC的时间开销



触发

    G1的新生代收集跟ParNew类似，当新生代占用达到一定比例的时候，开始出发收集。
    Young GC 回收的是所有年轻代的Region。当E区不能再分配新的对象时就会触发。
    E区的对象会移动到S区，当S区空间不够的时候，E区的对象会直接晋升到O区，
    同时S区的数据移动到新的S区，如果S区的部分对象到达一定年龄，会晋升到O区。
    

过程

    ，经过Young GC后存活的对象被复制到一个或者多个区域空闲中，这些被填充的区域将是新的新生代；
    当新生代对象的年龄(逃逸过一次Young GC年龄增加１)已经达到某个阈值(ParNew默认15)，被复制到老年代的区域中。
      
     回收过程是停顿的(STW,Stop-The-Word);
     回收完成之后根据Young GC的统计信息调整Eden和Survivor的大小，有助于合理利用内存，提高回收效率。
     
特点
     
     回收的过程多个回收线程并发收集。
     

# 老年代收集


Mixed GC
Mixed GC 翻译过来叫混合回收。之所以叫混合是因为回收所有的年轻代的Region+部分老年代的Region。


1、为什么是老年代的部分Region？
2、什么时候触发Mixed GC?
回收部分老年代是参数-XX:MaxGCPauseMillis，用来指定一个G1收集过程目标停顿时间，默认值200ms，当然这只是一个期望值。
G1的强大之处在于他有一个停顿预测模型（Pause Prediction Model），他会有选择的挑选部分Region，去尽量满足停顿时间，关于G1的这个模型是如何建立的，这里不做深究。
Mixed GC的触发也是由一些参数控制。
比如XX:InitiatingHeapOccupancyPercent表示老年代占整个堆大小的百分比，默认值是45%，达到该阈值就会触发一次Mixed GC。



流程
Mixed GC主要可以分为两个阶段：
1、全局并发标记（global concurrent marking）

初始标记
    
    首先初始标记(Initial-Mark),这个阶段是停顿的(Stop the World Event)，
    它标记了从GC Root开始直接可达的对象
    第一阶段initial mark是共用了Young GC的暂停，这是因为他们可以复用root scan操作，
    所以可以说global concurrent marking是伴随Young GC而发生的。
    
    
Root Region Scanning，

    程序运行过程中会回收survivor区(存活到老年代)，这一过程必须在young GC之前完成。
    
Concurrent Marking，

    并发标记（Concurrent Marking）。
    这个阶段从GC Root开始对heap中的对象标记，标记线程与应用程序线程并行执行，
    并且收集各个Region的存活对象信息。
    
    过程中还会扫描上文中提到的SATB write barrier所记录下的引用。
    在整个堆中进行并发标记(和应用程序并发执行)，此过程可能被young GC中断
    在并发标记阶段，若发现区域对象中的所有对象都是垃圾，那个这个区域会被立即回收(图中打X)

Remark, 

    再标记，会有短暂停顿(STW)。
    再标记阶段是用来收集 并发标记阶段 产生新的垃圾(并发阶段和应用程序一同运行)；
    G1中采用了比CMS更快的初始快照算法:snapshot-at-the-beginning (SATB)。     
     
Copy/Clean up，

    多线程清除失活对象，会有STW。
    G1将回收区域的存活对象拷贝到新区域，清除Remember Sets，
    并发清空回收区域并把它返回到空闲区域链表中。 
    清除垃圾（Cleanup，部分STW）。这个阶段如果发现完全没有活对象的region就会将其整体回收到可分配region列表中。 清除空Region。
    
    
 2、拷贝存活对象（Evacuation）
 
     Evacuation阶段是全暂停的。它负责把一部分region里的活对象拷贝到空region里去（并行拷贝），
     然后回收原本的region的空间。Evacuation阶段可以自由选择任意多个region来独立收集构成收集集合（collection set，简称CSet），CSet集合中Region的选定依赖于上文中提到的停顿预测模型，该阶段并不evacuate所有有活对象的region，只选择收益高的少量region来evacuate，这种暂停的开销就可以（在一定范围内）可控。
     
# Full GC
G1的垃圾回收过程是和应用程序并发执行的，当Mixed GC的速度赶不上应用程序申请内存的速度的时候，Mixed G1就会降级到Full GC，使用的是Serial GC。Full GC会导致长时间的STW，应该要尽量避免。
导致G1 Full GC的原因可能有两个：

Evacuation的时候没有足够的to-space来存放晋升的对象；
并发处理过程完成之前空间耗尽


