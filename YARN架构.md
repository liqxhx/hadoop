### 
### 参考
[hadoop2.8.2 YARN 架构](http://blog.csdn.net/maosijunzi/article/details/78476381)

YARN是为了解决各种不同集群(spark,Storm,hadoop)，在yarn平台上运行时资源分配的问题。</br>
各种集群程序之所以能在YARN上运行，是因为它们都实现了YARN的相应的规范。</br>

MapReduce放到YARN平台上，是如何运行的？</br>
YARN的ResourceManager接收到我们提交的MapReduce程序后，会在某个NodeManager上启动一个MRAppMaster进程，MRAppMaster负责MapRereduce任务的分配，在分配任务时，会先向ResourceManager请求资源(Container)，ResouceManager返回可以分配的NodeManager的连接信息(如ip)后，MRAppMaster会直接连接到相应的NodeManager上启动任务(至于是map任务还是reduce任务由MRAppMaster决定)。
