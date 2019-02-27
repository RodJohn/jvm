
 
    
# 基于栈的指令集 与 基于寄存器的指令集

    运算过程的中间变量都以操作数栈的入栈、出栈为信息交换途径

Java编译器输出的指令流，基本上是一种基于栈的指令集架构，指令流中的指令大部分都是零地址指令，他们依赖操作数栈进行工作。

基于栈的指令集主要优点就是可移植，使用栈架构指令集，用户程序不会直接使用寄存器，就可以由虚拟机实现来自行决定把一些访问最频繁的数据放到寄存器中以获取尽量好的的性能。

栈架构指令集的主要缺点是执行速度相对来说会稍慢一些，主要是因为指令数量和内存访问导致的。




# 示例

```
    public class Test {
        public  static int calc(){
            int i=100;
            int j=200;
            int k=300;
            return (i+j)*k;
        }
    
        public static void main(String[] args) {
            Test.calc();
        }
    }
```    

https://blog.csdn.net/owen1190/article/details/52328739
