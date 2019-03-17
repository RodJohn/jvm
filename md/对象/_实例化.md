

# 过程

	类加载
	内存分配
	初始化


	内存空间初始化：
	 虚拟机将分配到的内存空间都初始化为零值。
	 保证了对象的实例字段在java代码中可以不赋初始值就直接使用。
	
	对象头的设置：
	 虚拟机对对象进行必要的设置，
	 例如这个对象是哪个类的实例、如何才能找到类的元数据信息、对象的哈希码、对象的GC年代等信息。
	
	执行init方法


# 过程

没有父类的情况：

	1)类的静态属性
	2)类的静态代码块
	3)类的非静态属性
	4)类的非静态代码块
	5)构造方法

有父类的情况:

	1)父类的静态属性
	2)父类的静态代码块
	3)子类的静态属性
	4)子类的静态代码块
	5)父类的非静态属性
	6)父类的非静态代码块
	7)父类构造方法
	8)子类非静态属性
	9)子类非静态代码块
	10)子类构造方法


# 示例

## 1
代码
```
public class Singleton {
  private static Singleton singleton = new Singleton();
  public static int counter1;
  public static int counter2 = 0;
  private Singleton() {
      counter1++;
      counter2++;
  }
  public static Singleton getSingleton() {
      return singleton;
  }
 }

public class TestSingleton {
  public static void main(String args[]){
      Singleton singleton = Singleton.getSingleton();
      System.out.println("counter1="+singleton.counter1);
      System.out.println("counter2="+singleton.counter2);
  }
}
```
结果

	Singleton1 value1:1
	Singleton1 value2:0

解析
	
	Singleton.getSingleton()触发类的初始化
	
	准备
		singleton =null
		counter1 =0
		counter2 =0
		
	初始化工作。
		singleton = new Singleton();
			这样会执行构造方法内部逻辑，进行++；则counter1=1，counter2 =1 ；
		counter1 没有初始化代码
		counter2初始化为0

		一旦开始初始化静态部分，无论是否完成，后续都不会再重新触发静态初始化流程了。
因此在实例化st变量时，实际上是把实例初始化嵌入到了静态初始化流程中，并且在楼主的问题中，嵌入到了静态初始化的起始位置。这就导致了实例初始化完全至于静态初始化之前。

衍生

	改变顺序
	 public static int counter1;
	 public static int counter2 = 0;
 	 private static Singleton singleton = new Singleton();

	效果
	Singleton1 value1:1
	Singleton1 value2:1

## 2

代码

```
public class StaticTest
{
    public static void main(String[] args)
    {
        staticFunction();
    }

    static StaticTest st = new StaticTest();

    static
    {
        System.out.println("1");
    }

    {
        System.out.println("2");
    }

    StaticTest()
    {
        System.out.println("3");
        System.out.println("a="+a+",b="+b);
    }

    public static void staticFunction(){
        System.out.println("4");
    }

    int a=110;
    static int b =112;
}
```

结果

	2
	3
	a=110,b=0
	1
	4
