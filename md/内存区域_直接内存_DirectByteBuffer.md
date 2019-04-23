# 

	DirectByteBuffer 这个类是 JDK 提供使用堆外内存的一种途径


# 创建

## 源码

DirectByteBuffer

	DirectByteBuffer(int cap) 
	{			

	super(-1, 0, cap, cap, false);
	Bits.reserveMemory(cap);
	int ps = Bits.pageSize();
	long base = 0;
	try {
		base = unsafe.allocateMemory(cap + ps);
	} catch (OutOfMemoryError x) {
		Bits.unreserveMemory(cap);
		throw x;
	}
	unsafe.setMemory(base, cap + ps, (byte) 0);
	if (base % ps != 0) {
		// Round up to page boundary
		address = base + ps - (base & (ps - 1));
	} else {
		address = base;
	}
	cleaner = Cleaner.create(this, new Deallocator(base, cap));
		  viewedBuffer = null;
	}

reserveMemory

	static void reserveMemory(long size) 
	{

	synchronized (Bits.class) {
		if (!memoryLimitSet && VM.isBooted()) {
	  maxMemory = VM.maxDirectMemory();
	  memoryLimitSet = true;
		}
		if (size <= maxMemory - reservedMemory) {
	  reservedMemory += size;
	  return;
		}
	}

	// 显示调用垃圾回收
	System.gc();
	try {
		Thread.sleep(100);
	} catch (InterruptedException x) {
		// Restore interrupt status
		Thread.currentThread().interrupt();
	}
	synchronized (Bits.class) {
		if (reservedMemory + size > maxMemory)
	  throw new OutOfMemoryError("Direct buffer memory");
		reservedMemory += size;
	}

	}
  
 解析
 
 
 	首先向 Bits 类申请额度，
	Bits 类内部维护着当前已经使用的堆外内存值，
	会检查当前申请的大小与已经使用的内存大小是否超过总的堆外内存大小
	如果 check 不通过，
	会主动执行 System.gc()，然后 sleep 100 毫秒，再进行 check，如果内存还是不足，就抛出 OOM Error。
	如果 check 通过，
	就会调用 unsafe.allocateMemory 真正分配内存，返回内存地址，然后再将内存清 0。
	
	由于申请内存前可能会调用 System.gc()，
	所以谨慎设置 -XX:+DisableExplicitGC 这个选项，这个参数作用是禁止代码中显示触发的 Full GC。
	
# 回收

## 源码

DirectByteBuffer
	
	cleaner = Cleaner.create(this, new Deallocator(base, size, cap));
	
Cleaner

	public class Cleaner extends PhantomReference {
		private static final ReferenceQueue dummyQueue = new ReferenceQueue();
		private static Cleaner first = null;
		private Cleaner next = null;
		private Cleaner prev = null;
		private final Runnable thunk;

		private static synchronized Cleaner add(Cleaner var0) {
		   ...
		}

		private static synchronized boolean remove(Cleaner var0) {
			...
		}

		private Cleaner(Object var1, Runnable var2) {
			super(var1, dummyQueue);
			this.thunk = var2;
		}

		public static Cleaner create(Object var0, Runnable var1) {
			return var1 == null?null:add(new Cleaner(var0, var1));
		}

		public void clean() {
			if(remove(this)) {
				try {
					this.thunk.run();
				} catch (final Throwable var2) {
					AccessController.doPrivileged(new PrivilegedAction() {
						public Void run() {
							if(System.err != null) {
								(new Error("Cleaner terminated abnormally", var2)).printStackTrace();
							}

							System.exit(1);
							return null;
						}
					});
				}

			}
		}
	}
	
Deallocator

	private static class Deallocator
		implements Runnable
	{

		private static Unsafe unsafe = Unsafe.getUnsafe();

		private long address;
		private long size;
		private int capacity;

		private Deallocator(long address, long size, int capacity) {
			assert (address != 0);
			this.address = address;
			this.size = size;
			this.capacity = capacity;
		}

		public void run() {
			if (address == 0) {
				// Paranoia
				return;
			}
			unsafe.freeMemory(address);
			address = 0;
			Bits.unreserveMemory(size, capacity);
		}

	}
	
 ## 解析
 
	创建完DirectByteBuffer时，同时同时创建了一个Cleaner 	

	Cleaner 类，内部维护了一个 Cleaner 对象的链表，
	create(Object, Runnable) 方法创建 cleaner 对象，调用自身的 add 方法，将其加入到链表中。
	clean 方法首先将对象自身从链表中删除，保证只调用一次，然后执行 this.thunk 的 run 方法，
	thunk 就是由创建时传入的 Runnable 参数

	Deallocator 类的对象就是 DirectByteBuffer 中的 cleaner 传进来的 Runnable 参数类，
	un 方法 unsafe.freeMemory 释放内存，然后更新 Bits 里已使用的内存数据


# 自动回收

## 自动

	Cleaner是PhantomReference的子类
	PhantomReference的作用在于跟踪垃圾回收过程，并不会对对象的垃圾回收过程造成任何的影响。
	用DirectByteBuffer创建Cleaner对象，则会对当前对象的垃圾回收过程进行跟踪。
	当DirectByteBuffer对象从pending状态 ——> enqueue状态时，会触发Cleaner的clean()


## 手动

	手动回收，就是由开发手动调用 DirectByteBuffer 的 cleaner 的 clean 方法来释放空间
	Netty 中的堆外内存池就是使用反射来实现手动回收方式进行回收的。

 # 参考
 
	 https://www.jianshu.com/p/8e942cd7b572
	 https://www.jianshu.com/p/007052ee3773
 
  
