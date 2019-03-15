
# 通用配置

   java 
   -Xmx3550m -Xms3550m -Xmn2g -Xss128k 
   -XX:ParallelGCThreads=20 

   -XX:+UseConcMarkSweepGC -XX:+UseParNewGC
   -XX:+UseCMSCompactAtFullCollection -XX:CMSFullGCsBeforeCompaction=5 
   -XX:CMSInitiatingOccupancyFraction=70 -XX:+UseCMSInitiatingOccupancyOnly 
   -XX:+CMSScavengeBeforeRemark
   -XX：CMSClassUnloadingEnabled
   
   -XX:-+DisableExplicitGC

# 正常参数

CMSGC 24小时内一次  每次小于200毫秒

YGC  
频率 5-10秒 
时长 少于50ms
存活率  8% 

InitMark 老年代存活比例 简介反映碎片状态
Remark 老年代存活比例 反映饱和状态

堆内存 3G以上比较好

# CMSGC时间过长


## 情况一 ：ReScan时间过长

示例

	[Rescan (parallel) , 0.6762340 secs]

分析

	ReScan扫描时间过长，应该是新生代过多

处理

	开启Remark前强制YGC（-XX:+CMSScavengeBeforeRemark）

## SWAP 区域 

前提是你的服务器上是有 SWAP 区域（用 top、 vmstat 等命令可以看出）用于内存的换入换出，那么操作系统可能会将 JVM 中不活跃的内存页换到 SWAP 区域用以释放内存给线程使用（这也透露出内存开始不够用了）。内存换入换出是一个开销巨大的磁盘操作，比内存访问慢好几个数量级。

再看看此时的 vmstat 命令中 si、so 列的数值，如果数值大说明换入换出严重，这是内存不足的表现。

解决方法：减少线程，这样可以降低内存换入换出；增加内存；如果是 JVM 内存设置过大导致线程所用内存不足，则适当调低 -Xmx 和 -Xms。


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



大对象直接进老年代   



# 参考

https://www.cnblogs.com/Jaxlinda/p/7145912.html  定时任务
https://jjhpeopl.iteye.com/blog/2330447  excel对象过大

