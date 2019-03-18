

# 原理
    
    所有方法调用的目标方法在Class文件里面都是一个常量池中的符号引用：在类加载的解析阶段，会将其中一部分符号引用转化为直接引用。
    
    这种解析能成立的前提是：方法在程序真正运行前就有一个可确定的调用版本，并且这个方法的调用版本在运行期是不可改变的。
    换句话说，调用目标在程序代码写好，编译器进行编译时就必须确定下来。这类方法的调用称为解析
    
    
# 方法

    在java语言中符合“编译期可知，运行期不可变”这个要求的方法主要包括：
    
    静态方法，与类直接相关
    私有方法，在外部不可被访问
    这两种方法各自的特点决定了它们都不可能通过继承或别的方式重写其他版本，因此它们都适合在类加载阶段进行解析。
    
    
# 指令   
    
          JAVA虚拟机里面提供了5条方法调用字节码指令。分别如下：

      invokestatic:调用静态方法

      invokespecial:调用实例构造器<init>方法、私有方法和父类方法（super(),super.method()）。

      invokevirtual:调用所有的虚方法(静态方法、私有方法、实例构造器、父类方法、final方法都是非虚方法)。

      invokeinterface:调用接口方法，会在运行时期再确定一个实现此接口的对象。

      invokedynamic:现在运行时期动态解析出调用点限定符所引用的方法，然后再执行该方法，在此之前的4条指令，分派逻辑都是固化在虚拟机里面的，而invokedynamic指令的分派逻辑是由用户所设定的引导方法决定的。

     只要能被invokestatic和invokespecial指令调用的方法都可以在解析阶段中确定唯一的调用版本，符合这个条件的有静态方法、私有方法、实例构造器、父类方法4类，它们在类加载阶段就会把符号引用解析为该方法的直接引用。这些方法称为非虚方法（还包括使用final修饰的方法，虽然final方法使用invokevirtual指令调用，因为final方法注定不会被重写，也就是无法被覆盖，也就无需对其进行多态选择）。
     
 
# 示例

代码
  
    class Father {
        public static void print(String str) {
            System.out.println("father " + str);
        }

        private void show(String str) {
            System.out.println("father " + str);
        }
    }

    class Son extends Father {

    }

    public class Test {
        public static void main(String[] args) {
            Son.print("coder");
            //Father fa = new Father();
            //fa.show("cooooder");
        }
    }

运行结果

　　fatcher coder

　　说明：Son.print实际上调用的是Father的print方法，print方法与Father类型是相关的，

     
     
  
