
 # finalize
 
第一次标记
 
    如果对象被可达性分析算法判定死亡，它会被第一次标记
    然后JVM会判断对象是否有必要执行finalize()方法。

判定是否执行finalize

    执行finalize()方法的两个条件：
    1、重写了finalize()方法。
    2、finalize()方法之前没被调用过，因为对象的finalize()方法只能被执行一次。

执行finalize

    如果满足以上两个条件，这个对象将会放置在F-Queue的队列之中，
    并在稍后由一个虚拟机自建的、低优先级Finalizer线程来执行它finalize方法。
    
对象的“自我拯救”

    finalize()方法是对象脱离死亡命运最后的机会，
    如果对象在finalize()方法中重新与引用链上的任何一个对象建立关联即可，
    比如把自己（this关键字）赋值给某个类变量或对象的成员变量。


# 示例

代码

    public class FinalizeDemo {
      public static FinalizeDemo Hook = null;
      @Override
      protected void finalize() throws Throwable {
          super.finalize();
          System.out.println("执行finalize方法");
          FinalizeDemo.Hook = this;
      }
      public static void main(String[] args) throws InterruptedException {
          Hook = new FinalizeDemo();
          // 第一次拯救
          Hook = null;
          System.gc();
          Thread.sleep(500); // 等待finalize执行
          if (Hook != null) {
              System.out.println("我还活着");
          } else {
              System.out.println("我已经死了");
          }
          // 第二次，代码完全一样
          Hook = null;
          System.gc();
          Thread.sleep(500); // 等待finalize执行
          if (Hook != null) {
              System.out.println("我还活着");
          } else {
              System.out.println("我已经死了");
          }
      }
    }

结果

    执行finalize方法
    我还活着
    我已经死了

从结果可以看出，任何对象的finalize()方法都只会被系统调用一次。
不建议使用finalize()方法来拯救对象 ，原因如下：
1、对象的finalize()只能执行一次。
2、它的运行代价高昂。
3、不确定性大。
4、无法保证各个对象的调用顺序。

