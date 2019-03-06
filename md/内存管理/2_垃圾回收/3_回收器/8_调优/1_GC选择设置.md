

# 选择


响应优先

    -XX:+UseConcMarkSweepGC -XX:+UseParNewGC 
    -XX:CMSFullGCsBeforeCompaction=5 -XX:+UseCMSCompactAtFullCollection 

吞吐量优先

    -XX:+UseParallelGC –XX:ParallelGCThreads=20       



# 默认配置

client


Server

    新生代 Parallel
    年老代 Serial Old

mixed



# 手动设置

-XX:+UseSerialGC:

    新生代 Serial
    年老代 Serial Old
    
-XX:+UseParallelGC:设置并行收集器 
-XX:+UseParalledlOldGC:设置并行年老代收集器 

-XX:+UseConcMarkSweepGC:
    
    新生代 ParNew（可以使用-XX:+UseParNewGC选项来强制指定它）
    年老代 CMS

