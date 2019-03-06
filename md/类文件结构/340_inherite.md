

# 作用

	类索引用于确定这个类的全限定名，
	父类索引用于确定这个类的父类的全限定名。
	接口索引集合用来描述这个类实现了哪些接口。
	相当于确定这个类的继承实现关系。


# 示例

```

public interface UserMapper {
    void deleteById();
}
```

![](https://github.com/RodJohn/JVM/blob/master/img/ClassAccessFlag3.png)
![](https://github.com/RodJohn/JVM/blob/master/img/ClassAccessFlag4.png)

  类名UserMapper
  继承自Object
  无实现接口
  
