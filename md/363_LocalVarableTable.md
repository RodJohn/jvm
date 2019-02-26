
# 作用

    用于存放方法参数和方法内部定义的局部变量。

容量

    在 java程序编译为Class 文件时，
    就在方法的Code属性的max_locals 数据项中确定了该方法所需要分配的局部变量表的最大容量。

变量类型

    虚拟机使用局部变量表完成参数值到参数变量列表的传递过程的，
    局部变量表中第0位索引的Slot默认是用于传递方法所属对象实例的引用，
    在方法中可以通过关键字this来访问到这个隐含的参数。
    其余参数按照参数表顺序排序，
    占用从1开始的局部变量 Slot，参数表分配完毕后，
    再根据方法体内部定义的变量顺序和作用域分配其余的 Slot。
    
局部变量

    局部变量不存在准备阶段
    
    如果一个局部变量定义了但没有赋初始值是不能使用的，
    不要认为java中任何情况下都存在诸如整型变量默认为0，布尔变量默认为false等默认值。
 

# slot

    局部变量表容量是以变量槽（Slot）为最小单位，
    虚拟机规范中并没有明确指明一个Slot 应占用的内存空间大小，
    只是很有导向性地说到每个Slot都应该能够存放一个 boolean，byte，char，short，int，float，reference，returnAddress 类型的数据，
    这8种数据类型，都可以使用32位或更小的物理内存来存放。、
    它允许Slot的长度可以随着处理器、操作系统或虚拟机的不同而发生变化。
    
   
    
# Slot 重用

概念

    局部变量中，
    如果当前字节码PC计数器的值已经超出了某个变量的作用域，
    那这个变量对应的Slot 就可以交给其他变量使用了。
    这样可减少总栈帧空间 
    

示例和优化

```
public static void main(String[] args)() {
    byte[] placeholder = new byte[64 * 1024 * 1024];
    System.gc();
}

因为在执行System.gc() 时，变量placeholder 还处于作用域内，虚拟机自然不会回收其内存


public static void main(String[] args)() {
    {
        byte[] placeholder = new byte[64 * 1024 * 1024];
    }
    System.gc();
}

代码虽然已经离开了placeholder的作用域，但在此之后，没有任何对局部变量表的读写操作，placeholder原本所占用的Slot还没有被其他变量所复用，所以作为 GC Roots 一部分的局部变量仍然保持着对它的关联。


public static void main(String[] args)() {
    {
        byte[] placeholder = new byte[64 * 1024 * 1024];
    }
    int a = 0;
    System.gc();
}


public static void main(String[] args) {
    byte[] placeholder = new byte[64 * 1024 * 1024];
    placeholder = null;
    System.gc();
}

public static void main(String[] args) {
    show();
    System.gc();
}

private static void show() {
    byte[] placeholder =  new byte[64 * 1024 * 1024];
}

通过变量的作用域来控制

```

    以恰当的变量作用域来控制变量回收时间才是最优雅的解决方法。
        创建子方法-快速回收栈帧
    
    从执行角度讲：使用赋null值的操作来优化内存回收是建立在对字节码执行引擎概念模型的理解之上的；而赋null值的操作在经过 JIT 编译优化后就会被消除掉，这时候将变量设置为null就是没有意义的。
