## 基于gcplot进行gc日志分析

### gc日志的收集

[jvm参数配置参考地址](https://www.oracle.com/technetwork/java/javase/tech/vmoptions-jsp-140102.html)

gcplot推荐的gc日志收集jvm参数:

```java
-XX:+PrintGCDetails -XX:+PrintTenuringDistribution -XX:+PrintGCTimeStamps -XX:+PrintGCDateStamps -Xloggc:/path/to/file 
```

主要的参数项意义  Xloggc :指定gc日志的文件(1.8以后 推荐使用  -verbose )PrintGCDetails :打印回收详情, PrintGCTimeStamps :打印gc的时间戳， PrintHeapAtGC：打印gc的次数和gc前后的heap内存使用情况

> 有一点需要注意的是，如果直接指定gc日式文件，在程序重启后，之前记录的文件会被覆盖，导致内容丢失，在某些版本的Java中，可以在文件名中加上**％t**，这样文件名会带上当前时间格式字符串，或者**％p**带上进程ID，另外一种解决方案是基于内置**GC日志文件轮换** . `-XX：+ UseGCLogFileRotation -XX：NumberOfGCLogFiles = 10 -XX：GCLogFileSize = 10M` ， 通过最多10个文件，'.0'，'.1'... '.9'将被添加到您在Xloggc中给出的文件名中。 .0将是第一个，在达到.9之后，它将取代.0并以循环方式继续 

### 日志分析

#### gcplot的安装

[GCPLOT官网](https://gcplot.com/)

建议基于docker进行啊安装，非常方便

1. 安装docker 可以参见网上或者官网的教程
2. 终端运行命令 docker run -d -p 80:80 gcplot/gcplot
3. 在浏览器访问地址 http://127.0.0.1 用户名和密码均为admin

#### 使用

登陆后访问: General ->Upload gc log->选择文件->点击uplioad上传按钮。待上传成功后访问:Analysis Groups->Files-><你上传的日志文件> 就可以看到具体的分析信息



<img src="../../images/基于gcplot进行gc分析/1571745724767.png" alt="1571745724767" style="zoom:50%;" />

### gc分析

在该页面有多个tab，从多个维度分析了具体的gc信息主要介绍几个

#### Pause

包含新生代、老年代、方法区所的gc暂停所有信息，从STOP-THE-WORLD和 Concurrent 维度进行分析

重点可以看平均每次STW的时间和高消耗时间所占用的比例

![1571746621806](../../images/基于gcplot进行gc分析/1571746621806.png)

>  GC暂停的Log10（x），这对于查看低值的分布非常有帮助 

#### Generations

这是有关每个GC代的最详细的统计信息。有一些表，其中包含按代划分的总计，已占用的大小，以及累积的STW暂停图，最后还有大量统计信息，例如总计，最小/最大，平均暂停时间，事件之间的平均间隔等。

> 重点可以看看Full gc的次数、FULL GC间隔的时间、fullgc最小间隔时间以及总共STW的时间，

![image-20191024203102948](../../images/gcplot进行gc分析/image-20191024203102948.png)

另外堆内存，及各代平均使用大小，可以作为程序内存分配的参考

![image-20191024203219113](../../images/gcplot进行gc分析/image-20191024203219113.png)

#### tenuring stats

统计对象年龄的分布情况，并且会根据相关数据给出一定的建议，需要在jvm参数中加入PrintTenuringDistribution统计相关信息

#### system

分别统计内核CPU时间和进程内用户代码所花费的CPU时间(也就值gc的实际时间)

memory

分别统计 年轻代(yong)、老年代(tenured)、方法区(metaspace/perm)在gc前后的内存使用数据，并且还有数据经过EMA算法处理，能够看到相关数据比较平滑的曲线(尽量排除噪音数据的影响)

并且还有内存的提升速率和分配速率曲线

#### 分析日志概要

参考:
	[GC专家系列1：理解Java垃圾回收](https://segmentfault.com/a/1190000004233812)

​	《深入理解Java虚拟机》书籍

​	