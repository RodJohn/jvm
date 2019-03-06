
# 作用
	
	用于标识一些类层次的信息
	如：
	访问限制（是否为public类型）
	数据类型（Class是类.接口或者枚举注解）
	
# 特点

	编译器自动添加部分数据
	如：	
	接口会自动添加abstract
	动态代理类会添加SYNTHETIC

    
	
# 图例

![](https://github.com/RodJohn/JVM/blob/master/img/ClassAccessFlag1.png)




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
