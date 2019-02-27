
# 类加载器

## 概述

    通过类的全限定名来获取定义此类的二进制字节流、
    并转换成java.lang.Class类的一个实例；
    实现这个动作的代码模块称为“类加载器”。
    

## 类与加载器

    只有在满足如下三个类“相等”判定条件，才能判定两个类相等。
    1、两个类来自同一个Class字节流
    2、两个类是由同一个虚拟机加载
    3、两个类是由同一个类加载器加载

    
# 种类

## 系统提供

    Bootstrap ClassLoader：
        由C++实现，是虚拟机自身的一部分。
        负责将存放在lib目录并被虚拟机识别的类库加载到虚拟机内存中。
     
    Extension ClassLoader（扩展类加载器）：
        负责加载lib\ext\目录下的所有类库。
     
    Application ClassLoader（应用程序类加载器/系统类加载器）：
        负责加载用户类路径（ClassPaht）上所指定的类库，
        开发者可以直接使用这个类加载器，
        一般情况下也是程序的默认类加载器。
        
##	自定义

	除了系统提供的类加载器以外，开发人员可以通过继承 java.lang.ClassLoader类的方式实现自己的类加载器

```
public class FileSystemClassLoader extends ClassLoader { 
 
   private String rootDir; 
 
   public FileSystemClassLoader(String rootDir) { 
       this.rootDir = rootDir; 
   } 

   protected Class<?> findClass(String name) throws ClassNotFoundException { 
       byte[] classData = getClassData(name); 
       if (classData == null) { 
           throw new ClassNotFoundException(); 
       } 
       else { 
           return defineClass(name, classData, 0, classData.length); 
       } 
   } 
 
   private byte[] getClassData(String className) { 
       String path = classNameToPath(className); 
       try { 
           InputStream ins = new FileInputStream(path); 
           ByteArrayOutputStream baos = new ByteArrayOutputStream(); 
           int bufferSize = 4096; 
           byte[] buffer = new byte[bufferSize]; 
           int bytesNumRead = 0; 
           while ((bytesNumRead = ins.read(buffer)) != -1) { 
               baos.write(buffer, 0, bytesNumRead); 
           } 
           return baos.toByteArray(); 
       } catch (IOException e) { 
           e.printStackTrace(); 
       } 
       return null; 
   } 
 
   private String classNameToPath(String className) { 
       return rootDir + File.separatorChar 
               + className.replace('.', File.separatorChar) + ".class"; 
   } 
}
```
		
	类 FileSystemClassLoader继承自类 java.lang.ClassLoader。
	在 表 1中列出的 java.lang.ClassLoader类的常用方法中，一般来说，
	自己开发的类加载器只需要覆写 findClass(String name)方法即可。
	java.lang.ClassLoader类的方法 loadClass()封装了前面提到的代理模式的实现。
	该方法会首先调用 findLoadedClass()方法来检查该类是否已经被加载过；
	如果没有加载过的话，会调用父类加载器的 loadClass()方法来尝试加载该类；
	如果父类加载器无法加载该类的话，就调用 findClass()方法来查找该类。
	因此，为了保证类加载器都正确实现代理模式，在开发自己的类加载器时，最好不要覆写 loadClass()方法，而是覆写 findClass()方法。




# Class.forName

Class.forName(className)方法，
内部实际调用的方法是  Class.forName(className,true,classloader);
第2个boolean参数表示类是否需要初始化，  
一旦初始化，就会触发目标对象的 static块代码执行，static参数也也会被再次初始化。

    

ClassLoader.loadClass(className)方法，
内部实际调用的方法是  ClassLoader.loadClass(className,false);
第2个 boolean参数，表示目标对象是否进行链接，，
不进行链接意味着不进行包括初始化等一些列步骤，那么静态块和静态对象就不会得到执行


# 参考

https://www.ibm.com/developerworks/cn/java/j-lo-classloader/index.html
