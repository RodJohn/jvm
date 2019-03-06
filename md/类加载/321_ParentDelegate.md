
# 双亲委派
    
    如果一个类加载器收到了类加载的请求，
    首先自己不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，
    所有的加载请求最终都被传送到顶层的启动类加载器中，父类无法完成加载时，子类加载。
	
![](https://github.com/RodJohn/JVM/blob/master/img/ClassLoder.png)	

# 作用

唯一性

	确保每个加载器只能加载自己范围内的类；
    保证类的统一性

原理

    只有在满足如下三个类“相等”判定条件，才能判定两个类相等。
    1、两个类来自同一个Class字节流
    2、两个类是由同一个虚拟机加载
    3、两个类是由同一个类加载器加载


# ClassLoader

```

 private final ClassLoader parent;
 
 protected Class<?> loadClass(String name, boolean resolve)hrows ClassNotFoundException
   {
      synchronized (getClassLoadingLock(name)) {
          // First, check if the class has already been loaded
             Class c = findLoadedClass(name);
              if (c == null) {
                 long t0 = System.nanoTime();
                    try {
                         if (parent != null) {
                             c = parent.loadClass(name, false);
                          } else {
                             c = findBootstrapClassOrNull(name);
                               }
                           } catch (ClassNotFoundException e) {
                                // ClassNotFoundException thrown if class not found
                                // from the non-null parent class loader
                            }
                return c;
                   }
              }
```

	parent字段用于保存双亲

	JDK已经在loadClass方法中帮我们实现了ClassLoader搜索类的算法，
	当在loadClass方法中搜索不到类时，loadClass方法就会调用findClass方法来搜索类

	类 FileSystemClassLoader继承自类 java.lang.ClassLoader。
	在 表 1中列出的 java.lang.ClassLoader类的常用方法中，一般来说，
	自己开发的类加载器只需要覆写 findClass(String name)方法即可。
	java.lang.ClassLoader类的方法 loadClass()封装了前面提到的代理模式的实现。
	该方法会首先调用 findLoadedClass()方法来检查该类是否已经被加载过；
	如果没有加载过的话，会调用父类加载器的 loadClass()方法来尝试加载该类；
	如果父类加载器无法加载该类的话，就调用 findClass()方法来查找该类。
	因此，为了保证类加载器都正确实现代理模式，在开发自己的类加载器时，最好不要覆写 loadClass()方法，而是覆写 findClass()方法。

	
# Tomcat

	对于运行在 Java EE™容器中的 Web 应用来说，类加载器的实现方式与一般的 Java 应用有所不同。
	不同的 Web 容器的实现方式也会有所不同。
	以 Apache Tomcat 来说，每个 Web 应用都有一个对应的类加载器实例。
	该类加载器也使用代理模式，所不同的是它是首先尝试去加载某个类，如果找不到再代理给父类加载器。
	这与一般类加载器的顺序是相反的。
	这是 Java Servlet 规范中的推荐做法，其目的是使得 Web 应用自己的类的优先级高于 Web 容器提供的类。
    这种代理模式的一个例外是：Java 核心库的类是不在查找范围之内的。这也是为了保证 Java 核心库的类型安全。


   






