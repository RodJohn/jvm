
# 双亲委派

## 概念
    
    如果一个类加载器收到了类加载的请求，
    首先自己不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，
    所有的加载请求最终都被传送到顶层的启动类加载器中，父类无法完成加载时，子类加载。


## 实现代码

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


## 作用

	这样每个加载器只能加载自己范围内的类；
    就可以保证类的统一性

    


![](https://github.com/RodJohn/JVM/blob/master/img/ClassLoder.png)


# 破坏双亲委派

## 作用 

	若加载的基础类中需要回调用户代码，
	而这时顶层的类加载器无法识别这些用户代码，
	这时就需要破坏双亲委派模型了。 

	
JNDI破坏双亲委派模型 
	
	JNDI是Java标准服务，它的代码由启动类加载器去加载。但是JNDI需要回调独立厂商实现的代码，而类加载器无法识别这些回调代码（SPI）。 
	为了解决这个问题，引入了一个线程上下文类加载器。 可通过Thread.setContextClassLoader()设置。 
	利用线程上下文类加载器去加载所需要的SPI代码，即父类加载器请求子类加载器去完成类加载的过程，而破坏了双亲委派模型。

Spring破坏双亲委派模型 
	
	Spring要对用户程序进行组织和管理，而用户程序一般放在WEB-INF目录下，由WebAppClassLoader类加载器加载，而Spring由Common类加载器或Shared类加载器加载。 
	那么Spring是如何访问WEB-INF下的用户程序呢？ 
	使用线程上下文类加载器。 
	Spring加载类所用的classLoader都是通过Thread.currentThread().getContextClassLoader()获取的。
	当线程创建时会默认创建一个AppClassLoader类加载器（对应Tomcat中的WebAppclassLoader类加载器）： setContextClassLoader(AppClassLoader)。 
	利用这个来加载用户程序。即任何一个线程都可通过getContextClassLoader()获取到WebAppclassLoader。

## ThreadContextClassLoader

问题

	Java 提供了很多服务提供者接口（Service Provider Interface，SPI），允许第三方为这些接口提供实现。
	常见的 SPI 有 JDBC、JCE、JNDI、JAXP 和 JBI 等。
	这些 SPI 的接口由 Java 核心库来提供，如 JAXP 的 SPI 接口定义包含在 javax.xml.parsers包中。
	这些 SPI 的实现代码很可能是作为 Java 应用所依赖的 jar 包被包含进来，可以通过类路径（CLASSPATH）来找到，
	如实现了 JAXP SPI 的 Apache Xerces所包含的 jar 包。SPI 接口中的代码经常需要加载具体的实现类。
	如 JAXP 中的 javax.xml.parsers.DocumentBuilderFactory类中的 newInstance()方法用来生成一个新的 DocumentBuilderFactory的实例。
	这里的实例的真正的类是继承自 javax.xml.parsers.DocumentBuilderFactory，由 SPI 的实现所提供的。
	如在 Apache Xerces 中，实现的类是 org.apache.xerces.jaxp.DocumentBuilderFactoryImpl。

	而问题在于，SPI 的接口是 Java 核心库的一部分，是由引导类加载器来加载的；
	SPI 实现的 Java 类一般是由系统类加载器来加载的。
	引导类加载器是无法找到 SPI 的实现类的，因为它只加载 Java 的核心库。
	类加载器的代理模式无法解决这个问题。

概念

	线程上下文类加载器是从 JDK 1.2 开始引入的。
	类 java.lang.Thread中的方法 getContextClassLoader()和 setContextClassLoader(ClassLoader cl)用来获取和设置线程的上下文类加载器。
	默认情况下，线程将继承其父线程的上下文类加载器。
	Java 应用运行的初始线程的上下文类加载器是系统类加载器。
	
## Web 容器

	对于运行在 Java EE™容器中的 Web 应用来说，类加载器的实现方式与一般的 Java 应用有所不同。
	不同的 Web 容器的实现方式也会有所不同。
	以 Apache Tomcat 来说，每个 Web 应用都有一个对应的类加载器实例。
	该类加载器也使用代理模式，所不同的是它是首先尝试去加载某个类，如果找不到再代理给父类加载器。
	这与一般类加载器的顺序是相反的。
	这是 Java Servlet 规范中的推荐做法，其目的是使得 Web 应用自己的类的优先级高于 Web 容器提供的类。这种代理模式的一个例外是：Java 核心库的类是不在查找范围之内的。这也是为了保证 Java 核心库的类型安全。

## OSGi


