
# 概述

作用

	描述类级变量或实例级变量，
	不包括在方法内声明的变量
	不包括从父类或父接口继承的部分

结构

	由字段计数器和字段集合组成


# 字段格式

![](https://github.com/RodJohn/JVM/blob/master/img/ClassFileTable1.png)

## access_flags

	访问修饰符
	
	特有 volatile transient
	接口的字段会默认添加public 



![](https://github.com/RodJohn/JVM/blob/master/img/ClassFileTable2.png)


## name_index

	字段的简单名称
	指没有类型字段名称
	如变量 private final static int m 简单名称为 m。
  

		
## descriptor_index

	描述符用于描述字段的类型信息
	1同于常量池中的
	




## attribute

	字段的特殊属性如constvalue,signature

### constvalue

作用

	在类加载的连接过程会用于字段的第一次赋值

说明

	字段同时被final和static修饰，
	且数据类型是基本类型或String的话，
	
	编译时Javac将会为该常量生成ConstantValue属性，
	在类加载的准备阶段虚拟机便会根据ConstantValue为常量设置相应的值

	ConstantValue的属性值只限于基本类型和String，
	很明显这是因为它从常量池中也只能够引用到基本类型和String类型的字面量。
	

### signature

	记录泛型



# 示例

```
public class User {
    private static final int eyes = 2;
}
```

![](https://github.com/RodJohn/JVM/blob/master/img/ClassFileTable4.png)

![](https://github.com/RodJohn/JVM/blob/master/img/ClassFileTable5.png)
