# 分派调用             


    总所周知Java是一门面向对象的语言，因为Java具备的3个基本特征：继承、封装、多态。本点讲解的分派调用过程将会揭示多态性特征的一些基本体现，如“重载”和“重写”在JVM中是如何实现的。（此处的“实现”并非指语法，而是虚拟机如何确定正确的目标方法）


静态类型 动态类型





# 静态分派

概念

    依赖静态类型来定位方法执行版本的分派动作称为静态分派
    
特点
    
    静态分派发生在编译阶段，因此确定静态分派的动作实际上不是由虚拟机而是编译器执行的。
    
    编译器虽然能确定方法的重载版本，但很多情况下并不“唯一”，
    往往只能说是一个“更加适合”的版本。（
    产生这种模糊结论的主要原因是字母量不需要定义，
    所以字面量没有显式的静态类型，它的静态类型只能通过语言上的规则去理解和推断）


## 重载

关系

    重载是静态分派的一个体现

代码

```
public class StaticDispatch {

    static abstract class Human {
    }

    static class Man extends Human {
    }

    static class Woman extends Human {
    }

    public void sayHello(Human guy) {
        System.out.println("hello,guy!");
    }

    public void sayHello(Man guy) {
        System.out.println("hello,gentleman!");
    }

    public void sayHello(Woman guy) {
        System.out.println("hello,lady!");
    }

    public static void main(String[] args) {
        Human man = new Man();
        Human woman = new Woman();
        StaticDispatch sr = new StaticDispatch();
        sr.sayHello(man);
        sr.sayHello(woman);
        sr.sayHello(new Man());
        sr.sayHello(new Woman());

    }
}

```

结果

    hello,guy!
    hello,guy!
    hello,gentleman!
    hello,lady!

分析


    main()方法中对sayHello()进行调用，
    在方法接收者已经确定是 sr 的前提下，
    使用哪个重载版本，完全取决于传入参数的数量和数据类型。
    
## 模糊性

    
    

# 动态分派


概念

    在运行期根据实际类型确定方法执行版本的分派过程称为动态分派
    

## 重写


代码


```
public class DynamicDispatch {

    static abstract class Human {
        protected abstract void sayHello();
    }

    static class Man extends Human {
        @Override
        protected void sayHello() {
            System.out.println("man say hello");
        }
    }

    static class Woman extends Human {
        @Override
        protected void sayHello() {
            System.out.println("woman say hello");
        }
    }

    public static void main(String[] args) {
        Human man = new Man();
        Human woman = new Woman();
        man.sayHello();
        woman.sayHello();
        man = new Woman();
        man.sayHello();
    }
}
```

结果

```
man say hello
woman say hello
woman say hello
```

解析

图片
    
    编译时无法通过静态类型定位方法
    由于 invokevirtual指令执行的第一步就是在运行期确定接收者的实际类型，所以两次调用中的invokevirtual 指令把常量池中的类方法符号引用解析到了不同的直接引用上，这个过程就是 java语言中方法重写的本质。

```

```



# 单分派 与 多分派
  
  宗量：方法的接收者与方法的参数统称为方法的宗量；
  
  单分派和多分派：根据分派基于多少种宗量，可以将分派划分为单分派和多分派两种。单分派是根据一个宗量对目标方法进行选择，多分派是根据多于一个宗量对目标方法进行选择。
  
```
public class Dispatch {

    static class QQ {}

    static class _360 {}

    public static class Father {
        public void hardChoice(QQ arg) {
            System.out.println("father choose qq");
        }

        public void hardChoice(_360 arg) {
            System.out.println("father choose 360");
        }
    }

    public static class Son extends Father {
        public void hardChoice(QQ arg) {
            System.out.println("son choose qq");
        }

        public void hardChoice(_360 arg) {
            System.out.println("son choose 360");
        }
    }

    public static void main(String[] args) {
        Father father = new Father();
        Father son = new Son();
        father.hardChoice(new _360());
        son.hardChoice(new QQ());
    }
}
```

分析

    根据以上结果，来分析编译阶段编译器的选择过程，也就是静态分派过程。
    
    选择目标方法的依据：一是静态类型是Father还是Son，二是方法参数是QQ 还是360。
    
    这次选择结果的最终产物是产生两条invokevirtual指令：两条指令的参数分别是常量池中指向Father.hardChoice(360)以及Father.hardChoice(QQ) 方法的符号引用。因为是根据两个宗量进行选择，所以java语言的静态分派属于多分派类型。
    
    动态分派过程：在执行“son.hardChoice(new QQ())” 这句代码时，更准确地说，是在执行这句代码所对应的invokevirtual指令时，由于编译期已经决定目标方法的签名必须是 hardChoice(QQ)。虚拟机并不关心传递过来的参数，因为此时参数的静态类型、实际类型都对方法选择不会造成任何影响。唯一可以影响虚拟机选择的因素只有此方法的接收者的实际类型是Father还是Son。因为只有一个宗量作为选择依据，所以java语言的动态分派属于单分派类型。
    
    小结
    
    今天的 Java语言是一门 静态多分派、动态单分派的语言。按照目前java语言的发展趋势，它并没有直接变为动态语言的迹象，而是通过内置动态语言（如JavaScript）执行引擎的方式来满足动态性的需求
    
    

  
  
    
    
# 动态分派带来的缺陷
  
  由于动态分派是非常频繁的动作，而且动态分派的方法版本选择过程需要运行时在类的方法元数据中搜索合适的目标方法，因此在虚拟机的实际实现中基于性能的考虑，大部分实现都不会真正地进行如此频繁的搜索。
  
  虚方法表（Virtual Method Table）
  
  面对这种情况，最常用的稳定优化手段就是为类在方法区中建立一个虚方法表（Virtual Method Table），使用虚方法表索引来代替元数据查找以提高性能，查看以下代码清单所对应的虚方法表结构图：
 
