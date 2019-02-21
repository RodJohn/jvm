



# 参考

    深入理解Java虚拟街（JVM高级特性与最佳实践）
     实战Java虚拟机JVM故障诊断与性能优化
     深入理解JVM ＆ G1 GC
     揭秘Java虚拟机
    Java程序性能优化
    http://www.iocoder.cn/Books/Java-Virtual-Machine-books-recommended/
    
    
    
# 运行时数据区

[运行时数据区](https://github.com/RodJohn/JVM/blob/master/md/201_%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F.md)
[程序计数器](https://github.com/RodJohn/JVM/blob/master/md/202_%E7%A8%8B%E5%BA%8F%E8%AE%A1%E6%95%B0%E5%99%A8)
[虚拟机栈](https://github.com/RodJohn/JVM/blob/master/md/203_%E8%99%9A%E6%8B%9F%E6%9C%BA%E6%A0%88.md)
[本地方法栈](https://github.com/RodJohn/JVM/blob/master/md/204_%E6%9C%AC%E5%9C%B0%E6%96%B9%E6%B3%95%E6%A0%88.md)
[堆](https://github.com/RodJohn/JVM/blob/master/md/205_%E5%A0%86.md)
[方法区](https://github.com/RodJohn/JVM/blob/master/md/206_%E6%96%B9%E6%B3%95%E5%8C%BA.md)
[运行时常量池](https://github.com/RodJohn/JVM/blob/master/md/208_%E8%BF%90%E8%A1%8C%E6%97%B6%E5%B8%B8%E9%87%8F%E6%B1%A0.md)
[直接内存](https://github.com/RodJohn/JVM/blob/master/md/207_%E7%9B%B4%E6%8E%A5%E5%86%85%E5%AD%98.md)


    
    []()
    []()


# 内存分配
    
    策略
    
    本地线程缓存
    栈上分配
    堆上分代分配
    
    HSDB分析内存分配
    


# 内存管理


参考

    https://juejin.im/post/5b04d7aef265da0ba063834f
    


        
        
        内存溢出、


     


垃圾回收
    回收区域
        堆、方法区
         https://juejin.im/post/5c68fb65e51d4539a642594f
    存活算法
        引用计数算法
        可达性分析算法
        
        参考
            https://juejin.im/post/5c4520975188252620583972
    回收算法
   
        标记清除
        复制
        标记整理
        分代
        
        STW
            标记
            Safepoint
        
        
        参考
            https://juejin.im/post/5c498c9ae51d4506d613cb7a
    回收器
        分代收集器
            serial
            serial old
            ParNew
            Parallel Scavenge
            Parallel Old
            CMS
            顾吞吐量和停顿时间
         G1
            
         参考
            https://juejin.im/post/5c6378796fb9a049cd54b01d
            

# 类文件

    类文件结构
        常量池
        
    字节码指令
    
# 类加载

    加载时机
    
    加载过程
        加载
        验证
        解析
        准备
        https://www.cnblogs.com/ygj0930/p/6536048.html
        
        类的生命周期
        
    类加载器
       
        双亲委派模式
        


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



  
  

    

# JVM


    





# 参考

    JVM 作用 隔绝了物理
