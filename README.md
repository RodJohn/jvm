
# Index




<a href="#Class文件">Class文件</a>


<a href="#类加载">类加载</a>


# <a name="Class文件">Class文件</a>

 - [文件结构](https://github.com/RodJohn/JVM/blob/master/md/330_ClassFile.md)
 - - [魔数](https://github.com/RodJohn/jvm/blob/master/md/339_MagicNumber.md)
 - - [版本号](https://github.com/RodJohn/jvm/blob/master/md/337_VersionNumber.md)
 - - [静态常量池](https://github.com/RodJohn/jvm/blob/master/md/332_StaticConstantPool.md)
 - - [访问标识](https://github.com/RodJohn/jvm/blob/master/md/338_AccessFlag.md)
 - - [继承信息](https://github.com/RodJohn/jvm/blob/master/md/340_inherite.md)
 - - [字段表](https://github.com/RodJohn/jvm/blob/master/md/333_FiledTable.md)
 - - [方法表](https://github.com/RodJohn/jvm/blob/master/md/335_MethodTable.md)
 - - - [code](https://github.com/RodJohn/jvm/blob/master/md/336_Code.md)
 - [字节码指令]()
 - - []()
 
 # 字节码操作
 
 - [ASM]()
 - [JavaAgent]
 
 # 执行引擎

- [栈帧](https://github.com/RodJohn/JVM/edit/master/md/362_StackFrame.md)
- - [局部变量表](https://github.com/RodJohn/JVM/blob/master/md/363_LocalVarableTable.md)
- - [操作数栈、动态链接、返回地址](https://github.com/RodJohn/JVM/blob/master/md/364_OperandStack.md)
- [方法调用](https://github.com/RodJohn/JVM/blob/master/md/365_MethodCall.md)
- - [解析](https://github.com/RodJohn/JVM/blob/master/md/366_Resolution.md) 
- - [分派](https://github.com/RodJohn/JVM/blob/master/md/367_dispatch.md) 
- - [虚方法表](https://github.com/RodJohn/JVM/blob/master/md/368_MethodTable.md)
- 基于栈的解释执行引擎
- - [编译解析执行](https://github.com/RodJohn/JVM/blob/master/md/370_AnalyticalExecution.md)
- - [基于栈的解释执行引擎](https://github.com/RodJohn/JVM/blob/master/md/369_EngineOnSatck.md)
  
- [动态代理]()


# <a name="类加载">类加载</a>

- [类加载](https://github.com/RodJohn/jvm/blob/master/md/310_ClassLoad.md)
- - [加载阶段](https://github.com/RodJohn/JVM/blob/master/md/311_loadstage.md)
- - [链接阶段](https://github.com/RodJohn/JVM/blob/master/md/312_linkstage.md)
- - [初始化阶段](https://github.com/RodJohn/JVM/blob/master/md/313_initstage.md)
- - [加载时机](https://github.com/RodJohn/jvm/blob/master/md/314_LoadTime.md)
- [类加载器](https://github.com/RodJohn/JVM/blob/master/md/320_ClassLoder.md)
- - [双亲委派](https://github.com/RodJohn/jvm/blob/master/md/321_ParentDelegate.md)
- - [破坏双亲委派](https://github.com/RodJohn/jvm/blob/master/md/322_AntiParentDelegate.md)
- [热加载](https://github.com/RodJohn/jvm/blob/master/md/323_HotLoding.md)

  
    
    
# 运行时数据区

- [运行时数据区](https://github.com/RodJohn/JVM/blob/master/md/201_%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F.md)
- [程序计数器](https://github.com/RodJohn/JVM/edit/master/md/202_%E7%A8%8B%E5%BA%8F%E8%AE%A1%E6%95%B0%E5%99%A8.md)
- [虚拟机栈](https://github.com/RodJohn/JVM/blob/master/md/203_%E8%99%9A%E6%8B%9F%E6%9C%BA%E6%A0%88.md)
- [本地方法栈](https://github.com/RodJohn/JVM/blob/master/md/204_%E6%9C%AC%E5%9C%B0%E6%96%B9%E6%B3%95%E6%A0%88.md)
- [堆](https://github.com/RodJohn/JVM/blob/master/md/205_%E5%A0%86.md)
- [方法区](https://github.com/RodJohn/JVM/blob/master/md/206_%E6%96%B9%E6%B3%95%E5%8C%BA.md)
- -[运行时常量池](https://github.com/RodJohn/JVM/blob/master/md/208_%E8%BF%90%E8%A1%8C%E6%97%B6%E5%B8%B8%E9%87%8F%E6%B1%A0.md)
- -[运行时常量池实例](https://github.com/RodJohn/JVM/blob/master/md/209_%E8%BF%90%E8%A1%8C%E6%97%B6%E5%B8%B8%E9%87%8F%E6%B1%A0%E5%AE%9E%E4%BE%8B.md)
- [直接内存](https://github.com/RodJohn/JVM/blob/master/md/207_%E7%9B%B4%E6%8E%A5%E5%86%85%E5%AD%98.md)
- [内存错误](https://github.com/RodJohn/JVM/blob/master/md/210_%E5%86%85%E5%AD%98%E9%94%99%E8%AF%AF.md)

# 垃圾回收

- 存活判断
- - [引用计数法](https://github.com/RodJohn/jvm/blob/master/md/221_ReferenceCounting.md)
- - [可达性分析法](https://github.com/RodJohn/jvm/blob/master/md/222_ReachabilityAnalysis.md)
- 垃圾回收
- - [标记清除、复制、标记整理](https://github.com/RodJohn/jvm/blob/master/md/224_collction.md)
- - [分代收集](https://github.com/RodJohn/jvm/blob/master/md/229_GenerationCollection.md)
- STW
- - [HotSpot实现](https://github.com/RodJohn/jvm/blob/master/md/225_HotSpotImplement.md)



 - [垃圾回收器]()
 - - [新生代收集器](https://github.com/RodJohn/jvm/blob/master/md/230_YoungCollectors.md)
 - - [年老带收集器]()
 
 - [G1](https://github.com/RodJohn/JVM/blob/master/md/227_G1.md)
 - [finalize]()  
 
 - - [分区收集]()



引用类型
finalize


 
 # 对象分配
 
 - [对象分配]()
 -- [本地线程缓冲区分配]()
 -- [栈上分配]()
 -- [大对象直接进入老年代]()
 -- [分配在Eden中]()
 -- [长期存活的对象进入老年代]()
 -- [动态判定对象年龄]()
 -- [空间分配担保]()
 
 - [优化]()
 - [HSDB分析内存分配]()



     




# 优化

- - [javac编译器](https://github.com/RodJohn/JVM/blob/master/md/510_javac.md)
- - [语法糖](https://github.com/RodJohn/JVM/blob/master/md/514_Sugar.md)

- [即时编译](https://github.com/RodJohn/JVM/blob/master/md/520_JIT.md)
- - [HotSpots即时编译器](https://github.com/RodJohn/JVM/blob/master/md/522_HotSpotC1C2.md)
- - [公共表达式消除、方法内联、数组检测消除、栈上逃逸](https://github.com/RodJohn/JVM/blob/master/md/526_optimization.md)

# 调优
    
    can



# 并发

    内存模型
    
        https://www.jianshu.com/p/a463e23aedd5
    
    参考
        https://juejin.im/post/5c67d4f7e51d45403f2a9fd5
        https://www.hollischuang.com/archives/1003






# 参考

- 书籍
  * 《深入理解Java虚拟街（JVM高级特性与最佳实践）》 -- 周志明
  * 《实战Java虚拟机：JVM故障诊断与性能优化》 -- 葛一鸣
  * 《揭秘Java虚拟机》-- 封亚飞
  * 《深入理解JVM ＆ G1 GC》 -- 周明耀
  * 《HotSpot实战》 -- 陈涛


- 博客  
  * http://www.iocoder.cn/Books/Java-Virtual-Machine-books-recommended/
  * https://blog.csdn.net/u010349169/column/info/jvm-principle
 

# 起步
开始准备学习周志明的《深入理解Java虚拟机:JVM高级特性与最佳实践》 
同时学习对应的[视频学习](https://www.bilibili.com/video/av29502877)（适合每天4小时的地铁）

参考文章
https://blog.csdn.net/column/details/16384.html   （大佬）
https://www.cnblogs.com/royi123/tag/Java-JVM/
[郑雨迪的分享](https://time.geekbang.org/column/intro/108)

