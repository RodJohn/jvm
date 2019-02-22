



# 方案设计

代码
```

```

# 暂停进程

可以使用jdb调试并中断程序
当然使用IDE更方便


# 连接进程

## 查找进程

	使用jps命令查找虚拟机进程号

## 启动HSDB

启动命令

	java -classpath "%JAVA_HOME%/lib/sa-jdi.jar" sun.jvm.hotspot.HSDB

	启动完成后，将弹出
	
## 连接虚拟机进程

	选择
	输入虚拟机进程号

### 出错
	
现象

	如果用户界面一直连接不上
	同时命令行出现以下报错
	java -classpath "%JAVA_HOME%/lib/sa-jdi.jar" sun.jvm.hotspot.HSDB
	Exception in thread "Thread-1" java.lang.UnsatisfiedLinkError: Can't load library: C:\Program Files\Java\jre1.8.0_121\bin\sawindbg.dll
	        at java.lang.ClassLoader.loadLibrary(Unknown Source)
	        at java.lang.Runtime.load0(Unknown Source)
	        at java.lang.System.load(Unknown Source)
	        at sun.jvm.hotspot.debugger.windbg.WindbgDebuggerLocal.<clinit>(WindbgDebuggerLocal.java:661)
	        at sun.jvm.hotspot.HotSpotAgent.setupDebuggerWin32(HotSpotAgent.java:567)
	        at sun.jvm.hotspot.HotSpotAgent.setupDebugger(HotSpotAgent.java:335)
	        at sun.jvm.hotspot.HotSpotAgent.go(HotSpotAgent.java:304)
	        at sun.jvm.hotspot.HotSpotAgent.attach(HotSpotAgent.java:140)
	        at sun.jvm.hotspot.HSDB.attach(HSDB.java:1184)
	        at sun.jvm.hotspot.HSDB.access$1700(HSDB.java:53)
	        at sun.jvm.hotspot.HSDB$25$1.run(HSDB.java:456)
	        at sun.jvm.hotspot.utilities.WorkerThread$MainLoop.run(WorkerThread.java:66)
	        at java.lang.Thread.run(Unknown Source)


处理方案

	说明缺少sawindbg.dll，
	从C:\Program Files\Java\jdk1.8.0_121\jre\bin下面
	复制sawindbg.dll到C:\Program Files\Java\jre1.8.0_121\bin下，
	重新启动HSDB
		

# 内存分析



# 强力抄袭自

https://www.jianshu.com/p/a28ae76ac3b4
http://rednaxelafx.iteye.com/blog/1847971

可惜我操作不成功


https://mp.weixin.qq.com/s?__biz=MzI3MTAyNTQ2OA==&mid=2247484372&idx=1&sn=f06e129765bae7cb9b2821f3adf0ec2c&chksm=eac95787ddbede91e96a968113259cd0bfa60a7acff547eaa5d10a00888d7a159b335503e7b2&scene=21#wechat_redirect

https://mp.weixin.qq.com/s/XrHOmwShxH-9eZ-5ILL9aA

https://mp.weixin.qq.com/s/65Co4SdbLRm0qlzmuH8lzA

https://mp.weixin.qq.com/s/SJsa-rvwP-DiqxZQV1wEsA
