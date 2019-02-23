


# 概述

作用

	描述类的方法
	包括编译器自动添加的，
	 是类构造器“<clinit>”方法和实例构造器"<init>"方法。
	 子类重写的父类方法
  	不包括从父类或父接口继承的方法

结构

	由方法计数器和方法集合组成


	

# 结构

![]()

## access_flags

	记录方法的访问权限、静态or非静态、可变性、是否抽象等信息
	特殊 synchronized、native、strictfp 

![]()

## name_index

	方法的简单名称
	指没有类型或参数修饰的方法
	如变量 public static int getName()  简单名称为 getName() 。

## descriptor_index

	方法描述
	由参数列表（类型/顺序）和返回值组成
	基本格式为：(参数数据类型描述列表)返回值数据类型  
	如方法 int getIndex(String name,char[] tgc,int start,int end,char target)的描述符为“（Ljava/lang/String[CIIC）I”。

## attribute_info

	方法的实现被JVM编译成JVM的机器码指令，
	机器码指令就存放在一个Code类型的属性表中；
	方法声明要抛出异常，那么异常信息会在一个Exceptions类型的属性表中予以展现。


### code

	Java程序方法体中的代码经过Javac编译处理后，最终变为字节码指令存储在Code属性中。
	但并非所有方法表都有Code属性，例如抽象类或接口。
#### 结构
	
	1.attribute_name_index,属性名称索引，占有2个字节，其内的值指向了常量池中的某一项，该项表示字符串“Code”;
	2. attribute_length,属性长度，占有 4个字节，其内的值表示后面有多少个字节是属于此Code属性表的；
	3. max_stack,操作数栈深度的最大值，占有 2 个字节，在方法执行的任意时刻，操作数栈都不应该超过这个值，虚拟机的运行的时候，会根据这个值来设置该方法对应的栈帧(Stack Frame)中的操作数栈的深度；
	4. max_locals,最大局部变量数目，占有 2个字节，其内的值表示局部变量表所需要的存储空间大小；
	5. code_length,机器指令长度，占有 4 个字节，表示跟在其后的多少个字节表示的是机器指令；
	6. code,机器指令区域，该区域占有的字节数目由 code_length中的值决定。JVM最底层的要执行的机器指令就存储在这里；
	7. exception_table_length,显式异常表长度，占有2个字节，如果在方法代码中出现了try{} catch()形式的结构，该值不会为空，紧跟其后会跟着若干个exception_table结构体，以表示异常捕获情况；
	8. exception_table，显式异常表，占有8 个字节，start_pc,end_pc,handler_pc中的值都表示的是PC计数器中的指令地址。exception_table表示的意思是：如果字节码从第start_pc行到第end_pc行之间出现了catch_type所描述的异常类型，那么将跳转到handler_pc行继续处理。
	9. attribute_count,属性计数器，占有 2 个字节，表示Code属性表的其他属性的数目
	10. attribute_info,表示Code属性表具有的属性表，它主要分为两个类型的属性表：“LineNumberTable”类型和“LocalVariableTable”类型。
	“LineNumberTable”类型的属性表记录着Java源码和机器指令之间的对应关系
	“LocalVariableTable”类型的属性表记录着局部变量描述

#### 异常表

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

#### 重载

	在Java语言中，重载（Overload）一个方法，
	1、要与原方法具有相同的简单名称。
	2、要与原方法有不同的特征签名。
	 Java代码的方法特征签名只包括方法名称、参数顺序及参数类型；
	 而字节码的特征签名还包括方法返回值以及受查异常表。
	





### exception

	方法签名上的exception属性
	

### localvariableTable

