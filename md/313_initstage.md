

# 概述

    为类的静态变量赋予用户设定值
    也是执行类构造器<clinit>()
    
    
# clinit()

## 来源
    
    编译器自动收集类中的所有对类静态变量的操作（putstatic getstatic）
    如变量赋予初值，静态语句块中的语句
    

## 特点



父子
    
    <clinit>()方法与类的构造函数<init>()不同，
    不需要显式的调用父类构造器，虚拟机会保证父类的<clinit>()在子类的之前完成。
    由于父类<clinit>()方法先执行，也就意味着父类中定义的静态语句要优先于子类的变量赋值操作。

同步

    虚拟机会保证一个类的<clinit>()方法在多线程环境中正确的加锁同步，
    如果多个线程同时去初始化一个类，那么只会有一个线程去执行这个类的<clinit>()方法，
    其他线程都会阻塞，直到该方法执行完，
    
    如果在一个类的<clinit>()方法中有耗时很长的操作，可能会造成多个进程阻塞，
    在实际应用中，这种阻塞往往很隐蔽。
	
必须

    <clinit>()方法并不是必须的，如果一个类没有静态语句块也没有对变量赋值操作，就不会生成 
    
## 接口
    
    与类不同的是，接口的<clinit>()方法不需要执行父接口的<clinit>()方法。
    只有当父接口中定义的变量被使用时，父接口才初始化，
    接口的实现类在初始化时一样不会执行接口的<clinit>()方法。
   

# 初始化时机
	

有且只有

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
    
# 示例

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

Test init 
Super init 

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
