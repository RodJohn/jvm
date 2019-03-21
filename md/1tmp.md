


1991
  James Gosling领导开发Oak语言。
  
1995 
  Oak改名java
  Java语言第一次提出了“Write Once，Run Anywhere”的口号。
  
1996年
  Sun公司发布JDK 1.0
  提供了纯解释执行的Java虚拟机实现（Sun Classic VM）。  
  
1999年
  HotSpot虚拟机发布，
  HotSpot最初由一家名为“Longview Technologies”的小公司开发，因为HotSpot的优异表现，这家公司在1997年被Sun公司收购了。
  
2004年
  JDK 1.5发布
  在Java语法易用性上做出了非常大的改进
  
2006年

  JDK 1.6发布
　Sun公司宣布最终会将Java开源

2009年
  
  Oracle公司以74亿美元的价格收购Sun公司

2011年
  
  Oracle公司发布Java 1.7



Classic VM

  世界第一款商用Java虚拟机
  只能使用纯解释器方式来执行
  如果使用JIT编译器进行编译，解释器就会停止工作


Exact VM
  
  编译器与解析器混合工作
  准确式GC（虚拟机可以知道内存中的某个位置的数据具体是什么类型）


Hotspot VM

  使用最广泛的JVM
  编译器与解析器混合工作模式
  准确式GC
  热点代码探测技术（通过执行计数器找出最具有编译价值的代码，然后通知 JIT 编译器以方法为单位进行编译


JRockit

  号称世界上最快的Java 虚拟机。
  专注于服务器端应用，不太关注程序启动速度，
  内部不包含解析器实现，全部代码靠即时编译器编译后执行
  垃圾收集器和 MissionControl 服务套件等部分的实现，在众多 Java 虚拟机中也一直处于领先水平。


  Oracle 公司分别收购了 BEA 和 Sun，
  并在 JDK8 的时候，整合了 JRokit VM 和 HotSpot VM，
  如使用了 JRokit 的垃圾回收器与 MissionControl 服务，使用了 HotSpot 的 JIT 编译器与混合的运行时系统。





