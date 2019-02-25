



# 参考

    深入理解Java虚拟街（JVM高级特性与最佳实践）
     实战Java虚拟机JVM故障诊断与性能优化
     深入理解JVM ＆ G1 GC
     揭秘Java虚拟机
    Java程序性能优化
    http://www.iocoder.cn/Books/Java-Virtual-Machine-books-recommended/
    
    
    
# 运行时数据区

- [运行时数据区](https://github.com/RodJohn/JVM/blob/master/md/201_%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F.md)
- [程序计数器](https://github.com/RodJohn/JVM/edit/master/md/202_%E7%A8%8B%E5%BA%8F%E8%AE%A1%E6%95%B0%E5%99%A8.md)
- [虚拟机栈](https://github.com/RodJohn/JVM/blob/master/md/203_%E8%99%9A%E6%8B%9F%E6%9C%BA%E6%A0%88.md)
- [本地方法栈](https://github.com/RodJohn/JVM/blob/master/md/204_%E6%9C%AC%E5%9C%B0%E6%96%B9%E6%B3%95%E6%A0%88.md)
- [堆](https://github.com/RodJohn/JVM/blob/master/md/205_%E5%A0%86.md)
- [方法区](https://github.com/RodJohn/JVM/blob/master/md/206_%E6%96%B9%E6%B3%95%E5%8C%BA.md)
- - [运行时常量池](https://github.com/RodJohn/JVM/blob/master/md/208_%E8%BF%90%E8%A1%8C%E6%97%B6%E5%B8%B8%E9%87%8F%E6%B1%A0.md)
- - [运行时常量池实例](https://github.com/RodJohn/JVM/blob/master/md/209_%E8%BF%90%E8%A1%8C%E6%97%B6%E5%B8%B8%E9%87%8F%E6%B1%A0%E5%AE%9E%E4%BE%8B.md)
- [直接内存](https://github.com/RodJohn/JVM/blob/master/md/207_%E7%9B%B4%E6%8E%A5%E5%86%85%E5%AD%98.md)
- [内存错误](https://github.com/RodJohn/JVM/blob/master/md/210_%E5%86%85%E5%AD%98%E9%94%99%E8%AF%AF.md)

# 垃圾回收

 - [垃圾回收](https://github.com/RodJohn/JVM/blob/master/md/220_%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6.md)  
 - 存活判断算法
 - - [引用计数、可达性分析算法](https://github.com/RodJohn/JVM/edit/master/md/221_%E5%AD%98%E6%B4%BB%E7%AE%97%E6%B3%95.md)     
 - - [可达性分析法实现](https://github.com/RodJohn/JVM/blob/master/md/225_%E5%8F%AF%E8%BE%BE%E6%80%A7%E5%88%86%E6%9E%90%E6%B3%95%E5%AE%9E%E7%8E%B0.md)
 - 回收算法
 - - [标记清除、复制、标记整理算法](https://github.com/RodJohn/JVM/blob/master/md/224_%E5%9F%BA%E7%A1%80%E5%9B%9E%E6%94%B6%E7%AE%97%E6%B3%95.md)
 - - [分代收集算法](https://github.com/RodJohn/JVM/blob/master/md/229_%E5%88%86%E4%BB%A3%E6%94%B6%E9%9B%86%E7%AE%97%E6%B3%95.md)
 
 - [垃圾回收器](https://github.com/RodJohn/JVM/edit/master/md/226_%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E5%99%A8.md)
 - - [serial、parnew、parallel scravenge](https://github.com/RodJohn/JVM/blob/master/md/230_%E6%96%B0%E7%94%9F%E4%BB%A3%E6%94%B6%E9%9B%86%E5%99%A8.md)
 - - [serialOld、paralleOld、CMS](https://github.com/RodJohn/JVM/blob/master/md/231_%E8%80%81%E5%B9%B4%E4%BB%A3%E6%94%B6%E9%9B%86%E5%99%A8.md)
 
 - [G1](https://github.com/RodJohn/JVM/blob/master/md/227_G1.md)
 - [finalize]()  
 
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


# Class文件

 - [Class文件](https://github.com/RodJohn/JVM/blob/master/md/330_ClassFile.md)
 - - [魔数、版本号](https://github.com/RodJohn/JVM/blob/master/md/337_MagicVersion.md)
 - - [静态常量池](https://github.com/RodJohn/JVM/blob/master/md/332_StaticConstantPool.md)
 - - [访问标识、继承关系](https://github.com/RodJohn/JVM/blob/master/md/338_AccessFlag.md)
 - - [字段表](https://github.com/RodJohn/JVM/blob/master/md/333_FiledTable.md)
 - - [方法表](https://github.com/RodJohn/JVM/blob/master/md/335_MethodTable.md)
 - [字节码指令]()
 - - []()
 
    
# 类加载

- [类加载](https://github.com/RodJohn/JVM/blob/master/md/310_ClassLoad.md)
- - [加载阶段](https://github.com/RodJohn/JVM/blob/master/md/311_loadstage.md)
- - [链接阶段](https://github.com/RodJohn/JVM/blob/master/md/312_linkstage.md)
- - [初始化阶段](https://github.com/RodJohn/JVM/blob/master/md/313_initstage.md)
- [类加载器](https://github.com/RodJohn/JVM/edit/master/md/320_ClassLoder.md)
- - []()
- - []()

       

# 执行引擎
    
    运行时栈帧
    
    方法调用


# 优化

编译器优化

运行期优化


# 调优
    
    can



# 并发

    内存模型
    
        https://www.jianshu.com/p/a463e23aedd5
    
    参考
        https://juejin.im/post/5c67d4f7e51d45403f2a9fd5
        https://www.hollischuang.com/archives/1003





# 参考

    JVM 作用 隔绝了物理
