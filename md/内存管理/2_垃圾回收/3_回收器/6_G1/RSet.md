
# RSet

    空间换时间

# 结构
https://www.jianshu.com/p/870abddaba41


每个Region初始化时，会初始化一个remembered set（RSet）
该集合用来记录并跟踪其它Region指向该Region中对象的引用，
每个Region默认按照512Kb划分成多个Card，所以RSet需要记录的东西应该是 xx Region的 xx Card。

# 作用

进行垃圾回收时，如果Region1有根对象A引用了Region2的对象B，显然对象B是活的，
如果没有Rset，就需要扫描整个Region1或者其它Region，才能确定对象B是活跃的，
有了Rset可以避免对整个堆进行扫描。


在做YGC的时候，只需要选定young generation region的RSet作为根集，
这些RSet记录了old->young的跨代引用，避免了扫描整个old generation。 
而mixed gc的时候，
old generation中记录了old->old的RSet，young->old的引用由扫描全部young generation region得到
，这样也不用扫描全部old generation region。
所以RSet的引入大大减少了GC的工作量。

# 维护

https://www.jianshu.com/p/870abddaba41
    

# CSet
Collection Set（CSet），它记录了GC要收集的Region集合，集合里的Region可以是任意年代的。
在GC的时候，只要扫描对应的CSet中的RSet即可