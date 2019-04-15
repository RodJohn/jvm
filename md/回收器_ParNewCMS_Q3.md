

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
