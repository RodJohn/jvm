# 方向的设定

让新生代过滤掉更多的对象 CMS 碎片化

	12. 尽可能地让对象停留在新生代，因为新生代采用了复制算法，相对收回得更快，而且Minor GC的次数肯定比Full GC多，那么对象在新生代被清除的更能性会更高。而对象一旦进入到老年代，那么只有Full GC时才会回收，对象在整个系统停留的时间就会很长，很可能创建的它的线程早就死了，而它还活着


	13. 为了尽可能让对象停留在新生代，就要注意设置Survivor区域的大小，因为它直接和对象是否进入老年代相关。之前就遇到过这种情况，明明新生代还有很大的空间，但是每次Minor GC后总是有对象进入到了老年代。后来发现由于Survivor太小，导致Tenuring Threshold为1，意思是年龄为1的对象大小超过了Survivor / 2(可通过TargetSurvivorRatio来调节，默认是50，即1/2)，年龄只要超过1的对象这时候就要直接进入老年代了。而进入老年代，对象就只有在Full GC的时候才会被清除。而如果调大了Survivor空间，让对象对象尽量接近Max Tenuring Threshold时才进入到老年代，这时候会大大减少老年代的对象大小，并且让对象在新生代停留时间变长，提高了它们被快速清理出系统的概率。






    JVM参数调优是个很头痛的问题，设置的不好，
    JVM不断执行Full GC，导致整个系统变得很慢，网站停滞时间能达10秒以上，
    这种情况如果没隔几分钟就来一次，自己都受不了。
    这种停滞在测试的时候看不出来，只有网站pv达到数十万/天的时候问题就暴露出来了。
	
	
	
物理机

    1核  2G
    2核  4G
    4核  8G
    8核  16G 
    
    可用物理内存限制。32位系统下，一般限制在1.5G~2G；64为操作系统对内存无限制
	

FullGC的调节

    一些高性能、高并发的情况下，垃圾回收确成为了制约Java应用的瓶颈。目前JDK的垃圾回收算法，始终无法解决垃圾回收时的暂停问题，因为这个暂停严重影响了程序的相应时间，造成拥塞或堆积。这也是后续JDK增加G1算法

内存分配

    使用中等内存
    大内存容易造成fullgc的停顿时间过长
        12G fullgc 10多秒
        1.5G fullgc 400

运用

    中型机+docker 或者反向代理



	






YGC前后新生代变大
http://lovestblog.cn/blog/2016/05/18/ygc-worse/



# concurrent mode failure


新生代提升太快，老年代处理慢，老年代碎片太多
应用线程将会全部停止（相当于网站这段时间无法响应用户请求），进行压缩式垃圾收集（回退到 Serial Old 算法）

老年代碎片多
	0可以
	-XX:+UseCMSCompactAtFullCollection
	-XX:CMSFullGCBeforeCompaction=n    

老年代处理太慢
	
	降低CMS触发阈值 （CMSInitiatingOccupancyFraction）（70）
	并设置阈值固定  （UseCMSInitiatingOccupancyOnly）（很重要）

新生代提升过快

	
	如果频率太快的话，说明空间不足，首先可以尝试调大新生代空间和晋升阈值。
	0参看YGC频率 如果太快

业务性爆发

	定时任务或者线上活动
	业务活动

# Promotion Failure


老年代碎片太多，放不下大对象提升（表现为老年代还有很多空间但是，出现了 promotion failed）


	2016-01-07T18:54:26.948+0800: 18782.967: [GC2016-04-07T18:54:26.948+0800: 18782.967: [ParNew (promotion failed)
	Desired survivor size 117833728 bytes, new threshold 10 (max 10)
	- age   1:    6141680 bytes,    6141680 total
	- age   2:    6337936 bytes,   12479616 total
	- age   3:     549120 bytes,   13028736 total
	- age   4:      87768 bytes,   13116504 total
	- age   5:     221384 bytes,   13337888 total
	- age   6:     934168 bytes,   14272056 total
	- age   7:     146072 bytes,   14418128 total
	- age   8:     626064 bytes,   15044192 total
	- age   9:     398000 bytes,   15442192 total
	- age  10:     429616 bytes,   15871808 total
	: 1969227K->1929200K(2071808K), 0.7452140 secs]2016-01-07T18:54:27.693+0800: 18783.713: [CMS: 1394703K->632845K(2097152K), 4.0993640 secs] 3301676K->632845K(4168960K), [CMS Perm : 77485K->77473K(159744K)], 4.8450240 secs] [Times: user=5.18 sys=0.56, real=4.84 secs]
	Heap after GC invocations=5847 (full 7):
	 par new generation   total 2071808K, used 0K [0x00000006e9c00000, 0x0000000776400000, 0x0000000776400000)
	  eden space 1841664K,   0% used [0x00000006e9c00000, 0x00000006e9c00000, 0x000000075a280000)
	  from space 230144K,   0% used [0x0000000768340000, 0x0000000768340000, 0x0000000776400000)
	  to   space 230144K,   0% used [0x000000075a280000, 0x000000075a280000, 0x0000000768340000)
	 concurrent mark-sweep generation total 2097152K, used 632845K [0x0000000776400000, 0x00000007f6400000, 0x00000007f6400000)
	 concurrent-mark-sweep perm gen total 159744K, used 77473K [0x00000007f6400000, 0x0000000800000000, 0x0000000800000000)


 



# 参考

https://www.cnblogs.com/Jaxlinda/p/7145912.html  定时任务
https://jjhpeopl.iteye.com/blog/2330447  excel对象过大
https://blueswind8306.iteye.com/blog/1194438  完整优化
