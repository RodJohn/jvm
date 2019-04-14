
# 增量式算法

	增量式回收算法将标记阶段分为
	初始标记、并发标记、重新标记阶段
	最耗时并发收集不需要停顿用户线程


# 提前处理DirtyCard

DirtyCard

	并发标记使用三色标记法进行标记
	会将引用发生变化的对象Card标识为Dirty

提前处理

	在预清理和可中断的预清理阶段中
	会扫描DirtyCard，重新标记被被更新的对象


# 加快重新标记Rescan

重新标记
	
	重新标记的Rescan需要重新标记存活对象
	对于跨代引用采取的是遍历新生代对象
	如果能通过YGC减少新生代对象就可以加快扫描

触发YGC

	1.尝试在可中断性预清理
		拖延时间 尝试等待YGC
		YGC的频率一般是4秒
	2.强制YGC
		强制在CMS重新标记阶段之前进行YGC  
		参数-XX:CMSScavengeBeforeRemark



# 参考
https://www.jianshu.com/p/2a1b2f17d3e4

