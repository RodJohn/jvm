

# 触发条件

	老年代使用率达到阈值（周期性检查触发）
	新生代的晋升担保失败（立刻触发）
	永久代的使用率达到阈值(jdk8不考虑)



# 工作流程

![](https://github.com/RodJohn/JVM/blob/master/img/gccms.png)


# 初始标记（InitialMarking）

过程

	标记GCRoots能直接关联到的对象


特点

	独占式单线程执行，STW
	
  
# 并发标记(concurrent mark)

过程

	1、从初始标记获得的对象继续标记对象
	2、使用DirtyCard标记需要remark的数据
	
特点

	使用三色标记法进行标记
	
	
# Precleaning（预清理）

	通过参数CMSPrecleaningEnabled选择关闭该阶段，默认启用，主要做两件事情：

	处理新生代已经发现的引用，比如在并发阶段，在Eden区中分配了一个A对象，A对象引用了一个老年代对象B（这个B之前没有被标记），
	在这个阶段就会标记对象B为活跃对象。
	在并发标记阶段，如果老年代中有对象内部引用发生变化，
	会把所在的Card标记为Dirty（其实这里并非使用CardTable，而是一个类似的数据结构，叫ModUnionTalble），
	通过扫描这些Table，重新标记那些在并发标记阶段引用被更新的对象（晋升到老年代的对象、原本就在老年代的对象）




# 可中断的预清理(AbortablePreclean)

## 作用

	因为CMS GC的终极目标是降低垃圾回收时的暂停时间，所以在该阶段要尽最大的努力去处理那些在并发阶段被应用线程更新的老年代对象，
	这样在暂停的重新标记阶段就可以少处理一些，暂停时间也会相应的降低。

## 触发

	新生代Eden区的内存使用量大于阈值，
	如果新生代的对象太少，就没有必要执行该阶段   
	参数CMSScheduleRemarkEdenSizeThreshold 默认2M 超过2M就会触发


## 过程

	处理 From 和 To 区的对象，标记可达的老年代对象  
	和上一个阶段一样，扫描处理Dirty Card中的对象  
## 退出条件

	循环中断条件 
	最多循环的次数 CMSMaxAbortablePrecleanLoops，默认是0，意思没有循环次数的限制  
	最长执行时间 CMSMaxAbortablePrecleanTime，默认是5s
	Eden使用率 CMSScheduleRemarkEdenPenetration，默认50%，会退出循环。

 

  
# 重新标记(remark)

过程

    	修正并发标记期间因用户程序继续运作而导致标记变动的那一部分对象的标记记录

特点

	独占式并行执行（STW）
		
		
# 并发清除(concurrent sweep)
        （并发执行）
        
# 分析
        
    最耗费时间的标记与清除阶段都使用并发处理（不需要暂停工作）
    只有初始标记和重新标记需要STW，而且重新标记是并行（快速）
    所以整体的回收是低停顿的。


	

# 参考

http://www.importnew.com/27822.html  
https://www.jianshu.com/p/2a1b2f17d3e4
http://www.cnblogs.com/littleLord/p/5380624.html


