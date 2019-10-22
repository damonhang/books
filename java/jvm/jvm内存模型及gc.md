## 内存模型

### 运行时内存划分

 ![640?wx_fmt=png](https://ss.csdn.net/p?https://mmbiz.qpic.cn/mmbiz_png/nXyTmFfqCEMWCWQBibopnva05EERcRfiacZxWibdFcFBqWmeQPYS0y05ibPge8EhzCcISw0p8vglwgKd0ib227BZvaw/640?wx_fmt=png) 

#### heapdump

### GC

#### full gc

Full GC触发条件：
（1）调用System.gc时，系统建议执行Full GC，但是不必然执行
（2）老年代空间不足
（3）方法区空间不足
（4）通过Minor GC后进入老年代的平均大小大于老年代的可用内存
（5）由Eden区、From Space区向To Space区复制时，对象大小大于To Space可用内存，则把该对象转存到老年代，且老年代的可用内存小于该对象大小

#### gc log

