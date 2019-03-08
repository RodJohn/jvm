

# 标记清除算法

容器

	能够引用其他对象的对象都被称为容器（根除外）.
	因此只有容器之间才可能形成循环引用.
	

标记

 	给容器对象设置一个标记, 值引用计数值.

清除

  	对于每一个容器对象, 找到所有其引用的对象,将被引用对象的标记值减1.
	
	
找根

  	执行清除步骤以后所有标记值还大于0的都是存活对象
	都被非容器对象引用着, 至少存在一个非循环引用.

![](https://github.com/RodJohn/jvm/blob/master/img/%E5%BE%AA%E7%8E%AF%E5%BC%95%E7%94%A8%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88.png)
	
可达性分析

	存活对象的子孙对象也是存活对象
	无法到达的对象就是垃圾对象





# 参考

https://www.cnblogs.com/Leon-The-Professional/p/10137405.html  
http://python.jobbole.com/88827/
