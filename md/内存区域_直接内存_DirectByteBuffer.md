

DirectByteBuffer 这个类是 JDK 提供使用堆外内存的一种途径


# 创建

源码

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
	如果 check 不通过，会主动执行 System.gc()，然后 sleep 100 毫秒，再进行 check，如果内存还是不足，就抛出 OOM Error。
	如果 check 通过，就会调用 unsafe.allocateMemory 真正分配内存，返回内存地址，然后再将内存清 0。
	由于申请内存前可能会调用 System.gc()，所以谨慎设置 -XX:+DisableExplicitGC 这个选项，这个参数作用是禁止代码中显示触发的 Full GC。
	
# 回收


	
 
 # 参考
 
	 https://www.jianshu.com/p/8e942cd7b572
	 https://www.jianshu.com/p/007052ee3773
 
  
