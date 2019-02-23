


# 概述

作用

	全面描述类或者接口
	 比java语言的层面更加的严谨
	 比如 自动补全类和实例的构造方法，this字段的来源
   
 无关性
 
	实现JVM的语言无关性  
   
#  结构

## 结构

物理结构

    Class文件是以8位字节为基础单位的二进制流，
    数据间按顺序排列，中间没有添加任何分隔符。

逻辑结构

	Class文件格式只有两种数据类型：无符号数和表。

	无符号数属于基本的数据类型，
	以u1,u2,u4,u8来分别代表1个字节，2个字节，4个字节和8个字节的无符号数；
	可用来描述数字，索引引用，数量值或者按照UTF-8编码构成的字符串值。

	表是由多个无符号数或者其他表作为数据项构成的复合数据类型，所有表都习惯性地以“_info”结尾。
	表用于描述由层次关系的复合结构的数据，整个Class文件本质上就是一张表。



## 内容

内容由魔数、版本号、常量池、访问标示、继承结构、字段、方法组成
  ![在这里插入图片描述](https://img-blog.csdn.net/20181022110754745?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  






# 工具


WinHex16进制打开二进制流


javap -verbose 



# 系列





# 参考和图片来源


https://blog.csdn.net/luanlouis/article/category/2620885        
https://blog.csdn.net/A_zhenzhen/article/details/77977345
