


# 问题


https://www.jianshu.com/p/548c67aa1bc0


对于第一种情况，利用post-write barrier，记录所有新增的引用关系，然后根据这些引用关系为根重新扫描一遍
对于第二种情况，利用pre-write barrier，将所有即将被删除的引用关系的旧引用记录下来，最后以这些旧引用为根重新扫描一遍

## pre-write barrier

G1采用的是pre-write barrier解决这个问题。
简单说就是在并发标记阶段，当引用关系发生变化的时候
，通过pre-write barrier函数会把这种这种变化记录并保存在一个队列里，
在JVM源码中这个队列叫satb_mark_queue。在remark阶段会扫描这个队列，通过这种方式，
旧的引用所指向的对象就会被标记上，其子孙也会被递归标记上，这样就不会漏标记任何对象，
snapshot的完整性也就得到了保证。


其实只需要用pre-write barrier把每次引用关系变化时旧的引用值记下来就好了。这样，等concurrent marker到达某个对象时，
这个对象的所有引用类型字段的变化全都有记录在案，就不会漏掉任何在snapshot里活的对象。当然，很可能有对象在snapshot中是活的，但随着并发GC的进行它可能本来已经死了，但SATB还是会让它活过这次GC。
CMS的incremental update设计使得它在remark阶段必须重新扫描所有线程栈和整个young gen作为root；
G1的SATB设计在remark阶段则只需要扫描剩下的satb_mark_queue ，解决了CMS垃圾收集器重新标记阶段长时间STW的潜在风险。"


# SATB

SATB全称snapshot-at-the-beginning，由Taiichi Yuasa为增量式标记清除垃圾收集器开发的一个算法，
主要应用于垃圾收集的并发标记阶段，
解决了CMS垃圾收集器重新标记阶段长时间STW的潜在风险。

SATB的方式记录活对象，也就是那一时刻对象snapshot,
但是在之后这里面的对象可能会变成垃圾, 叫做浮动垃圾（floating garbage），这种对象只能等到下一次收集回收掉。
在GC过程中新分配的对象都当做是活的，其他不可达的对象就是死的。

解决了CMS垃圾收集器重新标记阶段长时间STW的潜在风险




# 标记新对象


https://www.jianshu.com/p/548c67aa1bc0


如何知道哪些对象是GC开始之后新分配的呢？
在Region中通过top-at-mark-start（TAMS）指针，分别为prevTAMS和nextTAMS来记录新配的对象。示意图如下：

https://www.jianshu.com/p/9e70097807ba

