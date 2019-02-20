



# 参考

    深入理解Java虚拟街（JVM高级特性与最佳实践）
    


# 概览


运行时内存区域

    程序计数器
    栈
    本地方法栈
    堆
    本地方法区
    特殊 
        常量池
        直接内存
        方法区、元空间

    参考
        https://juejin.im/post/5c3b1e5a51882522c03e812f
        https://juejin.im/post/5adefec26fb9a07ac85a0dea

对象创建

内存分配策略




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
            
            
            

调优
    
    can



调优和实战

类文件结构

类加载机制

字节码运行

代码编译和优化

并发

    内存模型
    
    参考
        https://juejin.im/post/5c67d4f7e51d45403f2a9fd5





    发展
    
  
  

    

# JVM


    

https://www.cnblogs.com/ygj0930/p/6536048.html



# 参考

    JVM 作用 隔绝了物理
