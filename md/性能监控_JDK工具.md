


# JDK监控工具

    jvm监控分析工具一般分为两类，一种是jdk自带的工具，一种是第三方的分析工具。
    jdk自带工具一般在jdk bin目录下面，以exe的形式直接点击就可以使用


# Jps

作用

    显示JVM进程列表
    包括本地虚拟机ID和主类名

参数

    -l 
        输出完整的main class或者jar
    -v
        输出传递给JVM的参数
常用

	jps -lv 
	
    
# jinfo

作用
    
    输出并修改运行时的java进程的参数
   
参数

    -sysprops
        查看系统参数
    -flags               
        打印JVM参数
    <no option>          
        表示不带任何选项,将打印出以上所提到的所有属性
    -flag <name>=<value> 
        设置指定参数的值

常用

	jinfo pid -- 列出JVM启动参数和system.properties
	
            
# jmap

作用

    打印JVM堆内对象情况
    
参数

    -heap
        打印heap的概要信息，
        如垃圾收集器组合、分带情况等
    -dump
        -dump:[live,]format=b,file=< filename>
        使用hprof二进制形式,输出jvm的heap内容到文件=. live子选项是可选的，假如指定live选项,那么只输出活的对象到文件。        

常用

	jmap -heap pid  -- 列出当前使用的GC算法，堆的各个区域大小

# jstack

作用

     打印堆栈信息
     称为threaddump或者javacore文件
     
    


# jstat

作用

    JVM监控统计信息（定时）

参数
    
    -class
    -gc
    -gccapacity

常用

	jstat -gc pid 1000 3  -- 列出堆的各个区域的大小
	jstat -gcutil pid 1000 3 -- 列出堆的各个区域使用的比例


   
# jconsole

作用

    图表化的形式显示各种数据。可通过远程连接监视远程的服务器VM。
    
使用方式

    jdk/bin目录下点击jconsole.exe即可启动

    
# jvisualvm

概述

    VisualVM 是
    jdk自带的最强大的调优工具
    可以使用插件拓展功能
    基于图形化界面的
    出现于JDK1.6
    
安装
    
    使用
        在jdk/bin目录下面双击jvisualvm.exe
        
    插件
        1、从主菜单中选择“工具”>“插件”。
        2、在“可用插件”标签中，选中该插件的“安装”复选框。单击“安装”。
        3、逐步完成插件安装程序。
    推荐插件
        Visual GC
        
        
# Visual GC

Graphs

    compile

    (1)编译时间有点长，用了3.794秒，这个时间主要是用来校验eclipse平台本身的字节码了，所以我们需要关闭字节码校验，让启动时候不会去校验平台本身（也是java写的）的字节码，为了达到这个目的，我们只需要在eclipse启动参数中加上-Xverify:none
    
    如下所示，因为我们用的是Spring Source Tool Suite,所以我们在STS.ini中增加这一行。
    
    类加载时间    
  
