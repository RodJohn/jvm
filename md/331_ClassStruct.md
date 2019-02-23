


# 魔数

概述

    class文件开始的四个字节叫做魔数
    值一定是十六进制0xCAFEBABE
    用于标记该文件是class文件

示例


![在这里插入图片描述](https://img-blog.csdn.net/20181022144732283?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

# 版本号

概述

 	用于标识class文件版本  
	版本号由主版本号和次版本号组成
	

作用

    高版本的JVM能向下兼容以前版本的Class文件，反之不行。
    JDK1.0的主版本号为45，
    以后的每个新主版本都会在原先版本的基础上加1,也就是45=1.1,46=1.2，


示例
![在这里插入图片描述](https://img-blog.csdn.net/20181022144942281?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)




# 访问标识

作用
	
	用于标识一些类层次的信息
	包括：是否为public类型、Class是类.接口或者枚举注解、是否为abstract类型
	其实叫做类标识符比较好

    
结构

	访问标识符是u2的数据，不同的字节代表不同的含义
![在这里插入图片描述](https://img-blog.csdn.net/20181022113035112?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


示例

```

public interface UserMapper {
    void deleteById();
}
```

![在这里插入图片描述](https://img-blog.csdn.net/20181023105551647?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
Classfile /C:/2_work/git_repository/tes/src/main/java/com/john/rod/UserMapper.class
  Last modified 2018-10-22; size 141 bytes
  MD5 checksum d898b2ef24a36d4e0eba82f310feaf93
  Compiled from "UserMapper.java"
public interface com.john.rod.UserMapper
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_INTERFACE, ACC_ABSTRACT
Constant pool:
  #1 = Class              #7              // com/john/rod/UserMapper
  #2 = Class              #8              // java/lang/Object
  #3 = Utf8               deleteById
  #4 = Utf8               ()V
  #5 = Utf8               SourceFile
  #6 = Utf8               UserMapper.java
  #7 = Utf8               com/john/rod/UserMapper
  #8 = Utf8               java/lang/Object
{
  public abstract void deleteById();
    descriptor: ()V
    flags: ACC_PUBLIC, ACC_ABSTRACT
}
SourceFile: "UserMapper.java"
```
	
	接口会默认添加abstract
	

	

# 类索引、父类索引与接口索引集合

作用

	类索引用于确定这个类的全限定名，
	父类索引用于确定这个类的父类的全限定名。
	接口索引集合用来描述这个类实现了哪些接口。

	类索引、父类索引、接口索引集合用于确定这个类的继承实现关系。

结构

	都是指向常量池的索引



示例

![在这里插入图片描述](https://img-blog.csdn.net/20181022161953714?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![在这里插入图片描述](https://img-blog.csdn.net/2018102216201329?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvZF9qb2hu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)





# 属性

	Class文件、字段表、方法表、属性表都可以携带自己的属性表集合，用于描述某些场景专有的信息



InnerClasses

	属性用于记录内部类与宿主类之间的关联。
