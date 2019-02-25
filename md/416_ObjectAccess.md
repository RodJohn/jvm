

# 访问方式

	对象的访问方式有句柄访问和直接指针访问。
	HotSpot采用直接指针的方式
	

## 句柄

原理

	Java堆中维护一个句柄池
	句柄中包含了对象实例数据和类型数据的地址
	reference中存储的就是对象的句柄地址

特点

	reference中存储的是稳定的句柄地址，
	在对象被移动时只要修改句柄中的实例数据指针，而reference本身不需要被修改。

![](https://github.com/RodJohn/JVM/blob/master/img/ObjectAccessMethod1.png)

## 直接指针

原理
	
	对象中包括实例数据和类型数据的地址
	reference中存储的就是对象的地址
	
特点

   	可以直接获取实例数据

![](https://github.com/RodJohn/JVM/blob/master/img/ObjectAccess2.png)
