# 虚引用 ( Phantom Reference )

	PhantomReference 是所有“弱引用”中最弱的引用类型。
	不同于软引用和弱引用，虚引用无法通过 get() 方法来取得目标对象的强引用从而使用目标对象，观察源码可以发现 get() 被重写为永远返回 null。


	虚引用主要被用来 跟踪对象被垃圾回收的状态，
	通过查看引用队列中是否包含对象所对应的虚引用来判断它是否 即将被垃圾回收，从而采取行动。
	它并不被期待用来取得目标对象的引用，
	而目标对象被回收前，它的引用会被放入一个 ReferenceQueue 对象中，从而达到跟踪对象垃圾回收的作用。
	当phantomReference被放入队列时，说明referent的finalize()方法已经调用，并且垃圾收集器准备回收它的内存了。
	注意：PhantomReference 只有当 Java 垃圾回收器对其所指向的对象真正进行回收时，会将其加入到这个 ReferenceQueue 对象中，这样就可以追综对象的销毁情况。这里referent对象的finalize()方法已经调用过了。
	所以具体用法和之前两个有所不同，它必须传入一个 ReferenceQueue 对象。当虚引用所引用对象准备被垃圾回收时，虚引用会被添加到这个队列中。
	
	nio中的堆外内存的回收就可以使用虚引用

	

# 示例

代码

	public class PhantomReferenceDemo {

		private static ReferenceQueue<MyObject> queue = new ReferenceQueue<>();

		public static void main(String[] args) throws InterruptedException {
			MyObject object = new MyObject();
			Reference<MyObject> phanRef = new PhantomReference<>(object, queue);
			System.out.println("创建的虚拟引用为 : " + phanRef);
			new Thread(new CheckRefQueue()).start();

			object = null;

			int i = 1;
			while (true) {
				System.out.println("第" + i++ + "次GC");
				System.gc();
				TimeUnit.SECONDS.sleep(1);
			}

		}

		public static class MyObject {

			@Override
			protected void finalize() throws Throwable {
				System.out.println("MyObject's finalize called");
				super.finalize();
			}

			@Override
			public String toString() {
				return "I am MyObject";
			}
		}

		public static  class CheckRefQueue implements Runnable {

			Reference<MyObject> obj = null;

			@Override
			public void run() {
				try {
					obj = (Reference<MyObject>)queue.remove();
					System.out.println("删除的虚引用为: " + obj + " , 获取虚引用的对象 : " + obj.get());
					System.exit(0);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
		}
	}


结果


	创建的虚拟引用为 : java.lang.ref.PhantomReference@1d44bcfa
	第1次GC
	MyObject's finalize called
	第2次GC
	删除的虚引用为: java.lang.ref.PhantomReference@1d44bcfa , 获取虚引用的对象 : null
	
分析
	

	第一次GC之后，
	系统找到了垃圾对象 object，并调用finalize()方法回收内存，但没有立即加入PhantomReference Queue中。
	因为MyObject对象重写了finalize()方法，并且该方法是一个非空实现，
	所以这里MyObject也是一个Final Reference。所以第一次GC完成的是Final Reference的事情。
	第二次GC时，
	该对象(即，MyObject)对象会真正被垃圾回收器进行回收，
	此时，将PhantomReference加入虚引用队列( PhantomReference Queue )。
	而且每次gc之间需要停顿一些时间，已给JVM足够的处理时间；如果这里没有TimeUnit.SECONDS.sleep(1); 可能需要gc到第5、6次才会成功。



# 参考

	https://www.jianshu.com/p/9a089a37f78d
