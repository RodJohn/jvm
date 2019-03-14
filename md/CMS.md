

https://www.jianshu.com/p/2a1b2f17d3e4

https://www.jianshu.com/p/03fac9502311

https://t.hao0.me/jvm/2016/03/15/jvm-gc-log.html

# 时间长短

Rescan

remark


其中可以看到CMS-initial-mark阶段暂停了0.0303050秒，而CMS-remark阶段暂停了0.0932010秒，因此两次暂停的总共时间是0.123506秒，也就是123毫秒左右。两次短暂停的时间之和在200以下可以称为正常现象。




https://blog.csdn.net/wenniuwuren/article/details/51131741


# Concurrent mode failed

Concurrent mode failed的产生是由于CMS回收年老代的速度太慢，导致年老代在CMS完成前就被沾满，引起full gc，避免这个现象的产生就是调小-XX:CMSInitiatingOccupancyFraction参数的值，让CMS更早更频繁的触发，降低年老代被沾满的可能。我们的应用暂时负载比较低，在生产环境上年老代的增长非常缓慢，因此暂时设置此参数为80。在压测环境下，这个参数的表现还可以，没有出现过Concurrent mode failed。


# Prommotion failed的日志输出大概是这样：
 [ParNew (promotion failed): 320138K->320138K(353920K), 0.2365970 secs]42576.951: [CMS: 1139969K->1120688K(
2166784K), 9.2214860 secs] 1458785K->1120688K(2520704K), 9.4584090 secs]

这个问题的产生是由于救助空间不够，从而向年老代转移对象，年老代没有足够的空间来容纳这些对象，导致一次full gc的产生。解决这个问题的办法有两种完全相反的倾向：增大救助空间、增大年老代或者去掉救助空间。增大救助空间就是调整-XX:SurvivorRatio参数，这个参数是Eden区和Survivor区的大小比值，默认是32，也就是说Eden区是Survivor区的32倍大小，要注意Survivo是有两个区的，因此Surivivor其实占整个young genertation的1/34。调小这个参数将增大survivor区，让对象尽量在survitor区呆长一点，减少进入年老代的对象。去掉救助空间的想法是让大部分不能马上回收的数据尽快进入年老代，加快年老代的回收频率，减少年老代暴涨的可能性，这个是通过将-XX:SurvivorRatio 设置成比较大的值（比如65536)来做到。在我们的应用中，将young generation设置成256M，这个值相对来说比较大了，而救助空间设置成默认大小(1/34)，从压测情况来看，没有出现prommotion failed的现象，年轻代比较大，从GC日志来看，minor gc的时间也在5-20毫秒内，还可以接受，因此暂不调整。

