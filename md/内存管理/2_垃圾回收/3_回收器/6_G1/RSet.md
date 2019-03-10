
# RSet

RSet（remembered set）
    
# 作用

RSet记录其它Region指向该Region中对象的引用  
RSet可以减少GC对堆的扫描。
    
# 原理

一般来说， 
gc首先枚举根节点。  
根节点有可能在新生代中，也有可能在老年代中。  
如果是YGC的话，没必要对位于老年代的GCRoots进行可达性分析。  
但确实可能存在位于老年代的某个 GC Root，它引用了新生代的某个对象  
而新生代的RememberedSet就能描述这种情况  
所以，“新生代的 GC Roots ” + “ 新生代RSet”，就能枚举出新生代真正的 GC Roots 。  

而mixed gc的时候，  
old generation中记录了old->old的RSet，young->old的引用由扫描全部young generation region得到  
，这样也不用扫描全部old generation region。  
所以RSet的引入大大减少了GC的工作量。  



# 维护

jvm通过异步队列来维护RSet，  

## 过程

G1在赋值动作的前后，  
jvm会插入一个pre-write barrier和post-write barrier，  

其中post-write barrier的最终动作如下：  
找到该字段所在的位置(Card)，并设置为dirty_card  
如果当前是应用线程，每个Java线程有一个dirty card queue，把该card插入队列  
除了每个线程自带的dirty card queue，还有一个全局共享的queue  

赋值动作到此结束，  
接下来的RSet更新操作交由多个ConcurrentG1RefineThread并发完成，每当全局队列集合超过一定阈值后，ConcurrentG1RefineThread会取出若干个队列，遍历每个队列中记录的card，并进行处理，位于G1RemSet::refine_card方法，大概实现逻辑如下：
1、根据card的地址，计算出card所在的Region
2、如果Region不存在，或者Region是Young区，或者该Region在回收集合中，则不进行处理
3、最终使用闭合函数G1UpdateRSOrPushRefOopClosure::do_oop_nv()的处理该card中的对象


# 参考


https://dsxwjhf.iteye.com/blog/2201685 （RSet作用）  
https://www.jianshu.com/p/870abddaba41  （RSet维护）  
