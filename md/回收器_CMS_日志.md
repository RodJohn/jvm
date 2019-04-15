

# 观察指标

初始标记时 老年代的使用率

	碎片率

重标记中 Rescan 时间

	是否需要提前处理



# 日志示例

## 初始标记 

示例 
  
    40.146: [GC [1 CMS-initial-mark: 26386K(786432K)] 26404K(1048384K), 0.0074495 secs]
说明

    使用CMS回收器进行老年代回收。
	老年代容量为786432K,CMS 回收器在空间占用达到 26386K 时被触发

## 并发标记 

示例

    40.154: [CMS-concurrent-mark-start]
    40.683: [CMS-concurrent-mark: 0.521/0.529 secs]
	
说明

    开始并发标记阶段
    并发标记阶段结束，占用 0.521秒CPU时间, 0.529秒墙钟时间(也包含线程让出CPU给其他线程执行的时间)

## 预清理 

示例

    40.683: [CMS-concurrent-preclean-start]
    40.701: [CMS-concurrent-preclean: 0.017/0.018 secs]
	
说明
    
    开始预清理阶段
    预清理阶段费时 0.017秒CPU时间，0.018秒墙钟时间。
	
## 可中断预清理

示例

    91834.123: [CMS-concurrent-abortable-preclean-start]
    91834.900: [CMS-concurrent-abortable-preclean: 0.769/0.777 secs] [Times: user=2.36 sys=0.53, real=0.78 secs]
    
说明

	开始可中断预清理
	预清理占用0.78秒
	

## 重新标记

示例

    40.704: [GC40.704: [Rescan (parallel) , 0.1790103 secs]
    40.883: [weak refs processing, 0.0100966 secs] [1 CMS-remark: 26386K(786432K)] 52644K(1048384K), 0.1897792 secs]

说明
    
    重新扫描费时0.1790103秒
    处理弱引用对象费时0.0100966秒，本阶段费时0.1897792 秒。



## 并发清理

示例

    40.894: [CMS-concurrent-sweep-start]
    41.020: [CMS-concurrent-sweep: 0.126/0.126 secs]

说明

    开始并发清理阶段，在清理阶段，应用线程还在运行。
    并发清理阶段费时0.126秒

## CMSScavengeBeforeRemark

示例

     2019-03-28T20:05:10.907+0800: 3644463.375: [GC (CMS Final Remark) 3644463.375: [ParNew (promotion failed): 434406K->315478K(1887488K), 5.8407801 secs] 3512406K->3486710K(5242112K), 5.8410096 secs] [Times: user=6.84 sys=1.31, real=5.84 secs]

说明

	CMSScavengeBeforeRemark开启了CMSRemark前的强制YGC 
    
    



# 参考
https://t.hao0.me/jvm/2016/03/15/jvm-gc-log.html

