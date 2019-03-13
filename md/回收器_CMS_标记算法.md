
# 并发标记

## 目的

为了减少标记过程造成的STW，进行了并发标记

## 影响

GC执行过程中，用户线程可能修改对象引用，会造成误标和漏标    
（如![三色标记法中的情况]()）

## 解决

变更记录
重新标记

# 变更记录

## 目的



## 过程

在并发标记阶段，
会将引用发生变化的对象Card标识为Dirty
在预清理和可中断的预清理阶段中  
会扫描DirtyCard，重新标记被被更新的对象


# 重新标记

## 过程

重新标记新生代和老年代

## 影响


## 解决

在并行预清理和重新标记之间发生一次YGC 可以大大减少年轻代数量 减少 重标记的工作量
可中断性预清理  
强制YGC减少新生代数量



# 可中断性预清理

## 作用

拖延时间 尝试等待YGC
YGC的频率一般是4秒
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
	


  
# 强制YGC

强制在CMS重新标记阶段之前进行YGC  
参数-XX:CMSScavengeBeforeRemark

	


# 参考
https://www.jianshu.com/p/2a1b2f17d3e4

