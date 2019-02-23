

# 概述

作用

	常量池中主要存放两大类常量：字面量和符号引用。
	
	字面量，
	 如基本数据类型、字符串、
	
	符号引用属于编译原理的概念，包括三类常量：
	 1，类和接口的全限定名；
	 2，字段的名称和描述符；
	 3，方法的名称和描述符。

结构
    
    常量池由常量池计数器、常量池集构成



#   计数

索引

	常量的索引值是从1而不是0开始的，
	索引值为0标示“不引用任何一个常量池项目”的含义。
   
计数值

    常量池计数器是常量总数数加一
    Long和Double型占用两个计数

	常量池计数器的值是10进制的17，这就代表了其中有16个常量，索引值范围为1-16。
 
   
# 常量元素 

## 结构

	常量元素由标志位和标志位对应的数据结构组成

类型

![在这里插入图片描述](https://img-blog.csdn.net/20181022151922338?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

结构

![在这里插入图片描述](https://img-blog.csdn.net/20181022152053429?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)




# 示例


 Class

	CONSTANT_Class_info型常量的结构中有一项name_index属性，
	该常属性中存放一个索引值，指向常量池中一个CONSTANT_Utf8_info类型的常量，
	该常量中即保存了该类的全限定名字符串。

Integer

	Java语言规范规定了 int类型和Float 类型的数据类型占用 4 个字节的空间。
	在常量池中，将 int类型的常量使用CONSTANT_Integer_info封装


符号引用

	而CONSTANT_Fieldref_info、CONSTANT_Methodref_info、CONSTANT_InterfaceMethodref_info型常量的结构中都有一项index属性，存放该字段或方法所属的类或接口的描述符CONSTANT_Class_info的索引项。

最终保存的诸如Class名、字段名、方法名、修饰符等字符串都是一个CONSTANT_Utf8_info类型的常量，

	

# 引用



## 符号引用和直接引用

符号引用
	
	符号引用以一组符号来描述所引用的目标，
	符号可以是任何形式的字面量，只要使用时能无歧义地定位到目标即可。
	符号引用与虚拟机实现的内存布局无关，引用的目标并不一定已经加载到了内存中。

直接引用

    直接引用可以是直接指向目标的指针、相对偏移量或是一个能间接定位到目标的句柄。
    直接引用是与虚拟机实现的内存布局相关的，
    同一个符号引用在不同虚拟机实例上翻译出来的直接引用一般不会相同。
    如果有了直接引用，那说明引用的目标必定已经存在于内存之中了。


## 动态连接

	虚拟机在加载Class文件时才会进行动态连接，
	也就是说，Class文件中不会保存各个方法和字段的最终内存布局信息，
	因此，这些字段和方法的符号引用不经过转换是无法直接被虚拟机使用的。
	当虚拟机运行时，需要从常量池中获得对应的符号引用，再在类加载过程中的解析阶段将其替换为直接引用
