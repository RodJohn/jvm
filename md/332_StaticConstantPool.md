
# 概述

	版本号之后是静态常量池计数器和常量项

# 作用

	常量池主要存放字面量和符号引用。

## 

	字面量(如基本数据类型、字符串)

符号引用

	就是用一段符号表示对应的引用
	符号引用属于编译原理的概念，包括三类常量：
	1，类和接口的全限定名；
	2，字段的名称和描述符；
	3，方法的名称和描述符。



# 常量池计数器

作用

	记录常量总数

特别规则

	普通常量的索引值是从1而不是0开始的，
	索引值为0标示“不引用任何一个常量池项目”
	常量池计数值包括索引值为0的标记
	
    

   
# 常量元素 
	

	常量元素由标志位和标志位对应的数据结构组成

类型

![](https://github.com/RodJohn/JVM/blob/master/img/StaticConstantPool1.png)

结构

![](https://github.com/RodJohn/JVM/blob/master/img/StaticConstantPool2.png)


# 示例

源码

	public class TestClass {
		private String name = "John";
		private int m;
		private int n = 2 ;

		public int inc() {
			return m + n;
		}
	}
	
Class文件

![](https://github.com/RodJohn/jvm/blob/master/img/StaticConstantPool6.png)

字节码分析

	通过javap -verbose TestClass.class的分析结果 
	
![](https://github.com/RodJohn/jvm/blob/master/img/StaticConstantPool5.png)

## 分析

常量池计数

	0X1C相当于28,刚好对应27个常量

常量项

	






