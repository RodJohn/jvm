


# 概述

作用

	描述类级变量或实例级变量，
	不包括在方法内声明的变量
	不包括从父类或父接口继承的部分

结构

	由字段计数器和字段集合组成

# 字段格式

![在这里插入图片描述](https://img-blog.csdn.net/20181022114218469?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## access_flags

	字段修饰符
	
	特有 volatile transient
	接口的字段会默认添加public 
		
![在这里插入图片描述](https://img-blog.csdn.net/20181022115439155?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)





## name_index

	字段的简单名称
	指没有类型字段名称
	如变量 private final static int m 简单名称为 m。
		
## descriptor_index

	描述符用于描述字段的数据类型
	
![在这里插入图片描述](https://img-blog.csdn.net/20181022115629343?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
	
	对于数组类型，
	每一维度将使用一个前置的“[”字符来描述，
	如一个整数数组“int [][]”将为记录为“[[I”，
	而一个String类型的数组“String[]”将被记录为“[Ljava/lang/String”



## attribute

	字段的特殊属性如constvalue,signature

### constvalue

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

![在这里插入图片描述](https://img-blog.csdn.net/20181022164643442?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![在这里插入图片描述](https://img-blog.csdn.net/20181022164718441?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)