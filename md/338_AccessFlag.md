
# 访问标识

作用
	
	用于标识一些类层次的信息
	包括：是否为public类型、Class是类.接口或者枚举注解、是否为abstract类型
	其实叫做类标识符比较好

    
结构

	访问标识符是u2的数据，不同的字节代表不同的含义

	
图例

![](https://github.com/RodJohn/JVM/blob/master/img/ClassAccessFlag1.png)



# 继承结构

# 类索引、父类索引与接口索引集合

作用

	类索引用于确定这个类的全限定名，
	父类索引用于确定这个类的父类的全限定名。
	接口索引集合用来描述这个类实现了哪些接口。

	类索引、父类索引、接口索引集合用于确定这个类的继承实现关系。

结构

	都是指向常量池的索引



# 示例

```

public interface UserMapper {
    void deleteById();
}
```

![](https://github.com/RodJohn/JVM/blob/master/img/ClassAccessFlag2.png)

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
	


![](https://github.com/RodJohn/JVM/blob/master/img/ClassAccessFlag3.png)
![](https://github.com/RodJohn/JVM/blob/master/img/ClassAccessFlag4.png)

