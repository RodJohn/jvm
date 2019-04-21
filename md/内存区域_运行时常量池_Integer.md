# 常规情况


赋值

Integer i1=40；
Java在编译的时候会直接将代码封装成Integer i1=Integer.valueOf(40);从而使用常量池中的对象。
Integer i1 = new Integer(40);
这种情况下会创建新的对象。

运算

语句i4 == i5 + i6，
因为+这个操作符不适用于Integer对象，首先i5和i6进行自动拆箱操作，进行数值相加，即i4 == 40。
然后Integer对象无法与数值进行直接比较，所以i4自动拆箱转为int值40，最终这条语句转为40 == 40进行数值比较。

示例

  @Test
  public void test1(){
      Integer i1 = 40;
      Integer i2 = 40;
      Integer i3 = 0;
      Integer i4 = new Integer(40);
      Integer i5 = new Integer(40);
      Integer i6 = new Integer(0);

      System.out.println("i1=i2   " + (i1 == i2));
      //true 
      System.out.println("i1=i2+i3   " + (i1 == i2 + i3));
      //true 
      System.out.println("i1=i4   " + (i1 == i4));
      //false
      System.out.println("i4=i5   " + (i4 == i5));
      //false        
      System.out.println("i4=i5+i6   " + (i4 == i5 + i6));
      //true 
      System.out.println("40=i5+i6   " + (40 == i5 + i6));
      //true 
  }


# 自定义设置

https://blog.csdn.net/chengzhezhijian/article/details/9628251
