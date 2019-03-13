

# 计数器溢出的问题

    计数器类型，在32位环境下是int在64位下是long，
    而指针是4字节对齐的
    即使所有对象都指向某一个对象，也是不会溢出的。

# 循环引用的问题

    将Python对象的引用计数器复制为记录值(根不是Python对象)
 
![](https://github.com/RodJohn/jvm/blob/master/img/%E5%BE%AA%E7%8E%AF%E5%BC%95%E7%94%A8%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%951.png)
 
 
    对Python对象执行部分标记清除算法
    

![](https://github.com/RodJohn/jvm/blob/master/img/%E5%BE%AA%E7%8E%AF%E5%BC%95%E7%94%A8%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%952.png)
 
    清除标记值为0的对象
