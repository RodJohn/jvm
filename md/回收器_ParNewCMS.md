# 正常参数

	堆内存 3G以上比较好  
	2核 4G 

	CMSGC 24小时内一次  每次小于200毫秒

	YGC  
	频率 5-10秒 
	时长 少于50ms
	存活率  8% 

	InitMark 老年代存活比例 简介反映碎片状态
	Remark 老年代存活比例 反映饱和状态


# 通用配置

    //JDK 版本 "1.8.0_45"
  
  	-Xms3550m -Xmx3550m 
	-XX:NewSize=1536m -XX:MaxNewSize=1536m 
	-XX:SurvivorRatio=8 
	-XX:MaxTenuringThreshold=9 
	-Xss256k
	-XX:MetaspaceSize=256m -XX:MaxMetaspaceSize=256m 

	-XX:PermSize=256m 
	-XX:MaxPermSize=256m 
	-XX:+CMSPermGenSweepingEnabled 
	-XX:CMSInitiatingPermOccupancyFraction=80
	-XX:+CMSClassUnloadingEnabled 


	-XX:+UseConcMarkSweepGC 
	-XX:+UseParNewGC
	-XX:CMSInitiatingOccupancyFraction=80 
	-XX:+UseCMSInitiatingOccupancyOnly 
	-XX:+UseCMSCompactAtFullCollection 
	-XX:CMSFullGCsBeforeCompaction=9 
	-XX:+CMSParallelRemarkEnabled 
	-XX:+CMSScavengeBeforeRemark 
	-XX:+ScavengeBeforeFullGC 
	-XX:ParallelGCThreads=20 


	-XX:+PrintGCDetails 
	-XX:+PrintGCDateStamps 
	-XX:+PrintGCApplicationConcurrentTime 
	-XX:+PrintGCApplicationStoppedTime
	-XX:+PrintHeapAtGC 
	-Xloggc:/data/applogs/heap_trace.txt 
	-XX:-HeapDumpOnOutOfMemoryError 
	-XX:HeapDumpPath=/data/applogs/HeapDumpOnOutOfMemoryError 



	-XX:-+DisableExplicitGC
	-XX:+ExplicitGCInvokesConcurrent
	-XX:+ExplicitGCInvokesConcurrentAndUnloadsClasses
	
	-XX:+IgnoreUnrecognizedVMOptions 



# 参数分析

堆

  	-Xms3550m -Xmx3550m 
	-XX:NewSize=1536m -XX:MaxNewSize=1536m 
	-XX:SurvivorRatio=8 
	-XX:MaxTenuringThreshold=9 
	-Xss256k
	

PermGen

	-XX:PermSize=256m 
	-XX:MaxPermSize=256m 					1.8移除永久代，会用metadata
	-XX:MetaspaceSize=256m 
	-XX:MaxMetaspaceSize=256m 
	-XX:+CMSPermGenSweepingEnabled 			这个参数在Java6中被移除了，因此需要下面的参数辅助
	-XX:+CMSClassUnloadingEnabled 			对永久代进行垃圾回收，可用设置标志
	-XX:CMSInitiatingPermOccupancyFraction 	设置Perm Gen使用到达多少比率时触发

CMS

	-XX:+UseConcMarkSweepGC 
	-XX:+UseParNewGC
	-XX:CMSInitiatingOccupancyFraction=80  回收阈值
	-XX:+UseCMSInitiatingOccupancyOnly     回收阈值固定
	-XX:+UseCMSCompactAtFullCollection     碎片压缩
	-XX:CMSFullGCsBeforeCompaction=9 	   碎片压缩频率、
	-XX:ParallelGCThreads=8 			   最大GC线程数
	-XX:+CMSParallelRemarkEnabled          CMSremark并行处理
	-XX:+ScavengeBeforeFullGC 			   OGC进行一次YGC，默认开启。
	-XX:+CMSScavengeBeforeRemark 		   CMSremark之前进行一次YGC






