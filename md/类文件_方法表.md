# 概述

作用

	描述类的方法
	包括编译器自动添加的(如。类构造器“<clinit>”方法和实例构造器"<init>"方法)
	子类重写的父类方法
  	不包括从父类或父接口继承的方法

结构

	由方法计数器和方法集合组成


	

# 结构

![]()

## access_flags

	记录方法的访问权限、静态or非静态、可变性、是否抽象等信息
	特殊 synchronized、native、strictfp 

![]()

## name_index

	方法的简单名称
	指没有类型或参数修饰的方法
	如变量 public static int getName()  简单名称为 getName() 。

## descriptor_index

	类似静态常量池中的NameAndType的描述

### 重载

	在Java语言中，重载（Overload）一个方法，
	1、要与原方法具有相同的简单名称。
	2、要与原方法有不同的特征签名。
	 Java代码的方法特征签名只包括方法名称、参数顺序及参数类型；
	 而字节码的特征签名还包括方法返回值以及受查异常表。
	

## attribute_info

	方法的实现被JVM编译成JVM的机器码指令，
	机器码指令就存放在一个Code类型的属性表中；
	方法声明要抛出异常，那么异常信息会在一个Exceptions类型的属性表中予以展现。

# code属性

![Code](https://github.com/RodJohn/jvm/blob/master/md/336_Code.md)

## exception

	方法签名上的exception属性
    \也就是 throws 关键字后列表的异常，
	





