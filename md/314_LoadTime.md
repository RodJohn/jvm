

# 加载时机

## 预加载

	Java虚拟机有预加载功能。
	类加载器并不需要等到某个类被"首次主动使用"时再加载它,JVM规范规定JVM可以预测加载某一个类，
	如果这个类出错，但是应用程序没有调用这个类， JVM也不会报错；
	如果调用这个类的话，JVM才会报错，（LinkAgeError错误)。
	其实就是一句话，Java虚拟机有预加载功能。
  
  
## 混合

	加载和连接阶段的部分内容是交叉进行的
    虚拟机规范并未规定解析阶段发生的具体时间，
    只要求了在执行操作符号引用的字节码指令之前，先对它们使用的符号引用进行解析。
    可以在类被加载器加载的时候对常量池的符号引用进行解析（也就是初始化之前），还是等到一个符号引用被使用之前进行解析（也就是在初始化之后）。	
 
# 初始化时机
	

	JVM仅有5种情况下会触发类的初始化	
	
作为启动类

	当虚拟机启动时，用户需要指定一个主类（包含main()方法的类），
	虚拟机会首先初始化这个类，（也叫主动加载）。

new

    使用new字节码指令创建类的实例，

static

    读取或设置一个静态字段的值（无constantValue属性）
    调用一个静态方法的时候，
    
        
作为父类

	当初始化一个类的时候，如果发现其父类没有进行过初始化，则首先触发父类初始化。
	（接口除外）


动态语言

    使用jdk1.7的动态语言支持时，
    如果一个java.lang.invoke.MethodHandle实例最后的解析结果REF_getStatic、REF_putStatic、RE_invokeStatic的方法句柄，
    并且这个方法句柄对应的类没有进行初始化，则需要先触发其初始化。
	
	
# Class.forName

	Class.forName(className)方法，
	内部实际调用的方法是  Class.forName(className,true,classloader);
	第2个boolean参数表示类是否需要初始化，  
	一旦初始化，就会触发目标对象的 static块代码执行，static参数也也会被再次初始化。



	ClassLoader.loadClass(className)方法，
	内部实际调用的方法是  ClassLoader.loadClass(className,false);
	第2个 boolean参数，表示目标对象是否进行链接，，
	不进行链接意味着不进行包括初始化等一些列步骤，那么静态块和静态对象就不会得到执行


	
    
# 示例

	-XX:+TraceClassLoading参数可以跟踪显示类加载

one

```
public class Super {
    public static int value = 2;
    static {
        System.out.println("Super init ");
    }
}

public class Suber extends Super {
    static {
        System.out.println("Suber init ");
    }
}

public class T6 {
    static {
        System.out.println("Test init ");
    }
    public static void main(String[] args) {
        int value = Suber.value;
    }
}

[Loaded test.T6 from file:/C:/Users/rod_j/Documents/workspace-spring-tool-suite-4-4.1.2.RELEASE/test/bin/]
[Loaded sun.launcher.LauncherHelper$FXHelper from C:\Program Files\Java\jre1.8.0_201\lib\rt.jar]
[Loaded java.lang.Class$MethodArray from C:\Program Files\Java\jre1.8.0_201\lib\rt.jar]
[Loaded java.lang.Void from C:\Program Files\Java\jre1.8.0_201\lib\rt.jar]
Test init 
[Loaded test.Super from file:/C:/Users/rod_j/Documents/workspace-spring-tool-suite-4-4.1.2.RELEASE/test/bin/]
[Loaded test.Suber from file:/C:/Users/rod_j/Documents/workspace-spring-tool-suite-4-4.1.2.RELEASE/test/bin/]
Super init 
[Loaded java.lang.Shutdown from C:\Program Files\Java\jre1.8.0_201\lib\rt.jar]
[Loaded java.lang.Shutdown$Lock from C:\Program Files\Java\jre1.8.0_201\lib\rt.jar]

对于访问静态字段，只有直接定义这个字段的类才被初始化，
因此通过子类来引用父类中定义的静态字段，只会触发父类的初始化而不会触发子类的初始化。

```




two
```
将Super中devalue改为static final
则Super和Suber都不初始化

常量在编译阶段会存入调用类的常量池，
本质上没有直接引用到定义常量的类，
因此不会触发定义常量的类的初始化。

将Super中devalue改为new Random().nextInt(100);
不是常量赋值就不会shenche

```
three

```
public class T6 {
    public static void main(String[] args) {
        Super[] supers = new Super[10];
    }
}

通过数组定义引用类，不会触发此类的初始化。
```
