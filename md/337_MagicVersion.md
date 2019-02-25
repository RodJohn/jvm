# 魔数

概述

    class文件开始的四个字节叫做魔数
    值一定是十六进制0xCAFEBABE
    
作用

    用于标记该文件是class文件

示例

![](https://github.com/RodJohn/JVM/blob/master/img/MagicNumber.png)


# 版本号

概述

    版本号由主版本号和次版本号组成
    用于标识class文件版本  

	

作用

    高版本的JVM能向下兼容以前版本的Class文件，反之不行。
    JDK1.0的主版本号为45，
    以后的每个新主版本都会在原先版本的基础上加1,也就是45=1.1,46=1.2，


示例

![](https://github.com/RodJohn/JVM/blob/master/img/VersionNumber.png)
