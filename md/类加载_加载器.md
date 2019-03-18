
# 作用

    通过类的全限定名来获取定义此类的二进制字节流、
    并转换成java.lang.Class类的一个实例；
    实现这个动作的代码模块称为“类加载器”。
    
    
# 种类

Bootstrap ClassLoader

	由C++实现，是虚拟机自身的一部分。
	负责将存放在lib目录并被虚拟机识别的类库加载到虚拟机内存中。

Extension ClassLoader（扩展类加载器）

	负责加载lib\ext\目录下的所有类库。

Application ClassLoader（应用程序类加载器/系统类加载器）

	负责加载用户类路径（ClassPaht）上所指定的类库，
	开发者可以直接使用这个类加载器，
	一般情况下也是程序的默认类加载器。

自定义

	开发人员可以通过继承 java.lang.ClassLoader类的方式实现自己的类加载器		



# 参考

https://www.ibm.com/developerworks/cn/java/j-lo-classloader/index.html
