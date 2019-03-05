# 双亲委派弊端


  类加载的委托方向是单向的
	若加载的基础类中需要回调用户代码，
	而这时顶层的类加载器无法识别这些用户代码，
	这时就需要破坏双亲委派模型了。 

	Java 提供了很多服务提供者接口（Service Provider Interface，SPI），允许第三方为这些接口提供实现。
	常见的 SPI 有 JDBC、JCE、JNDI、JAXP 和 JBI 等。
	这些 SPI 的接口由 Java 核心库来提供，如 JAXP 的 SPI 接口定义包含在 javax.xml.parsers包中。
	这些 SPI 的实现代码很可能是作为 Java 应用所依赖的 jar 包被包含进来，可以通过类路径（CLASSPATH）来找到，
	如实现了 JAXP SPI 的 Apache Xerces所包含的 jar 包。SPI 接口中的代码经常需要加载具体的实现类。
	如 JAXP 中的 javax.xml.parsers.DocumentBuilderFactory类中的 newInstance()方法用来生成一个新的 DocumentBuilderFactory的实例。
	这里的实例的真正的类是继承自 javax.xml.parsers.DocumentBuilderFactory，由 SPI 的实现所提供的。
	如在 Apache Xerces 中，实现的类是 org.apache.xerces.jaxp.DocumentBuilderFactoryImpl。


# ThreadContextClassLoader

	线程上下文类加载器是从 JDK 1.2 开始引入的。
	类 java.lang.Thread中的方法 getContextClassLoader()和 setContextClassLoader(ClassLoader cl)用来获取和设置线程的上下文类加载器。
	默认情况下，线程将继承其父线程的上下文类加载器。
	Java 应用运行的初始线程的上下文类加载器是系统类加载器。


	Spring要对用户程序进行组织和管理，而用户程序一般放在WEB-INF目录下，由WebAppClassLoader类加载器加载，而Spring由Common类加载器或Shared类加载器加载。 
	那么Spring是如何访问WEB-INF下的用户程序呢？ 
	使用线程上下文类加载器。 
	Spring加载类所用的classLoader都是通过Thread.currentThread().getContextClassLoader()获取的。
	当线程创建时会默认创建一个AppClassLoader类加载器（对应Tomcat中的WebAppclassLoader类加载器）： setContextClassLoader(AppClassLoader)。 
	利用这个来加载用户程序。即任何一个线程都可通过getContextClassLoader()获取到WebAppclassLoader。



# 
