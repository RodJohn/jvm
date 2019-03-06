
# 概述

	Java程序方法体中的代码经过Javac编译处理后，最终变为字节码指令存储在Code属性中。
	但并非所有方法表都有Code属性，例如抽象类或接口。
  
# 结构

![](https://github.com/RodJohn/JVM/blob/master/img/ClassFileCodeAttribute.png)
	
## max_stack
 
## max_locals

    attribute_name_index是一项指向CONSTANT_Utf8_info型常量的索引，常量值固定为Code。
    attribute_length指示了属性的长度。
    max_stack代表了操作数栈深度的最大值，在方法执行的任意时刻，操作数栈不会大于这个深度。
    max_locals代表了局部变量所需要的存储空间。max_locals单位是slot，slot是虚拟机为局部变量分配内存所使用的最小单位。
    对于byte,char,float,int,short,boolean,reference,return Address 等长度不超过32位的数据类型，每个局部变量使用1个slot，
    而double和long这两种64位数据类型则使用2个slot。
    注意，slot可以重用，当代码执行超出一个局部变量的作用域时，这个局部变量所占用的slot就可以被其它局部变量使用。
    code_length和code用于存储Java源程序编译后生成的字节码指令。
    code_length代表字节码长度，code用于存储字节码指令的一系列字节流。
    code由u1表示，虚拟机讲到一个字节码时就知道怎么理解，后续带什么参数等等。
    u1的取值是0到255，也就是说一共可以表达255条指令。
    code有点类似cpu上的指令集，例如+号会被编译成 iadd 虚拟机字节码指令。
    code_length由一个u4表示，理论上最大值是232-1，但虚拟机规范中限制方法不能超过65535。



## 异常表

作用

    异常表就是方法中的异常处理内容，try catch代码块

结构

![](https://github.com/RodJohn/JVM/blob/master/img/ClassFileCodeAttribute2.png)

    如果字节码从start_pc行到end_pc行之间出现类型为catch_type或其子类的异常，
    则转到handler_pc行继续处理。


	编译器使用异常表而不是简单的跳转命令来实现Java异常及finally处理机制；
	在JDK1.4.2之前的Javac编译器采用了jsr和ret指令实现finally语句，
	但在1.4.2之后已经改为编译器自动在每段可能的分支路径之后都将finally语句块的内容冗余生成一遍来实现finally语义；
	在1.7中已经完全禁止jsr和ret指令，如果遇到这两条指令，虚拟机会在类加载的字节码校验阶段抛出异常

```
package org.fenixsoft.clazz;
 
public class TestClass {
    public int inc() {     
        int x;
        try{
            x = 1;
            return x;
        }catch(Exception e){
            x = 2;
            return x;
        }finally{
            x = 3;
        }
    }
}

```

```
Code:
   Stack=1, Locals=5, Args_size=1
  
   0:   iconst_1  //try块中的x=1
   1:   istore_1  
   2:   iload_1   //保存x到returnValue中，此时x=1
   3:   istore  4  
   5:   iconst_3  //finaly块中的x=3
   6:   istore_1  
   7:   iload   4 //将returnValue中的值放到栈顶，准备给ireturn返回
   9:   ireturn  //返回方法的int元素（返回栈顶元素1）
   10:  astore_2  //给catch中定义的Exception e赋值，存储在Slot 2中
   11:  iconst_2  //catch块中的x=2
   12:  istore_1  
   13:  iload_1  //保存x到returnValue中，此时x=2
   14:  istore  4 
   16:  iconst_3 //finally块中的x=3
   17:  istore_1 
   18:  iload   4 //将returnValue中的值放到栈顶，准备给ireturn返回
   20:  ireturn  //返回方法的int元素（返回栈顶元素2）
 
   21:  astore_3 //如果出现了不属于Exception及其子类的异常才会走到这里
   22:  iconst_3 //finally块中的x=3
   23:  istore_1 
   24:  aload_3 //将异常放置到栈顶
   25:  athrow //抛出异常
  Exception table:
   from   to  target type
    5    10   Class java/lang/Exception   //第0到第5行如果抛出Exception异常则跳转到第10行
    5    21   any  //第0到第5行如果抛出任何异常则跳转到第21行
   16    21   any  //第10到第16行如果抛出任何异常则跳转到第21行

```
	编译器为这段Java源码生成了3条异常表记录，对应3条可能出现的代码执行路径。从Java代码的语义上讲，这3条执行路径分别为：
	如果try语句块中出现属于Exception或其子类的异常，则转到catch语句块处理。
	如果try语句块中出现不属于Exception或其子类的异常，则转到finally语句块处理。
	如果catch语句块中出现任何异常，则转到finally语句块处理。
	
	字节码0~4行所做的操作数就是将整数1赋值给变量x，并将此时x的值复制一份到最后一个本地变量表的Slot中（这个Slot里面的值在ireturn指令执行前将会被重新读到栈顶，作为方法返回值使用，这里暂且就记为returnValue）。如果这时没有出现异常，则会继续走到第5~9行，将变量x赋值为3，然后将之前保存到returnValue中的整数1读入到操作栈顶，最后ireturn指令会以int形式放回操作栈顶中的值，方法结束。如果出现了异常，PC寄存器指针转到第10行，第10~20行所做的事情是将2赋值给变量x，然后将变量x此时的赋值给returnValue，最后再将变量x的值改为3。方法返回前同样将returnValue中保留的整数2读到操作栈顶。从第21行开始的代码，作用是将变量x的值赋为3，并将栈顶的异常抛出，方法结束。



## localvariableTable
    
    LocalVariableTable用于描述栈桢中局部变量表中的变量与Java源码中定义的变量之间的关系
