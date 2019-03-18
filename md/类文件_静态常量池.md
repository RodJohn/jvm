
# 概述

	版本号之后是静态常量池计数器和常量项

# 作用

	常量池主要存放字面量和符号引用。

## 常量类型

字面量

	如final修饰的基本数据类型、字符串

符号引用

	就是代表对应的引用的数据结构
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

# 常用

NameAndType

	对于数组类型，
	每一维度将使用一个前置的“[”字符来描述，
	如一个整数数组“int [][]”将为记录为“[[I”，
	而一个String类型的数组“String[]”将被记录为“[Ljava/lang/String”

![](https://github.com/RodJohn/JVM/blob/master/img/ClassFileTable3.png)  

	方法描述
	由参数列表（类型/顺序）和返回值组成
	基本格式为：(参数数据类型描述列表)返回值数据类型  
	如方法 int getIndex(String name,char[] tgc,int start,int end,char target)的描述符为“（Ljava/lang/String[CIIC）I”。

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

第一个常量

	tag值为0X0A相当于10,
	表示常量类型为MethodRef（方法）
	根据MethodRef的定义,
	则0X07表示方法所属类的符号引用是第六个常量（Object）
	0X15表示方法描述对应第二十一个常量(init)






