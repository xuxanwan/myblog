---
layout: post
title:  "Java新手问题 05 GC"
date:   2017-07-09
categories: Java
comments: true
---

### 关于垃圾回收（Garbage Collection）
Java内存管理是Java语言最大特性之一,他允许开发者创建使用对象的时候无需考虑分配和释放，因为垃圾回收(GC)会自动回收利用内存空间。
- GC 是如何判断哪些对象已经失效？
    1. 引用计数算法(已被淘汰的算法)
        > 给对象中添加一个引用计数器,每当有一个地方引用它时,计数器值就加1;当引用失效时,计数器值就减1;任何时刻计数器为0的对象就是不可能再被使用的。目前主流的java虚拟机都摒弃掉了这种算法，最主要的原因是它很难解决对象之间相互循环引用的问题。尽管该算法执行效率很高。
    2. 可达性分析算法
        > 目前主流的编程语言(java,C#等)的主流实现中,都是称通过可达性分析(Reachability Analysis)来判定对象是否存活的。这个算法的基本思路就是通过一系列的称为“GC Roots”的对象作为起始点,从这些节点开始向下搜索,搜索所走过的路径称为引用链(Reference Chain),当一个对象到GC Roots没有任何引用链相连(用图论的话来说,就是从GC Roots到这个对象不可达)时,则证明此对象是不可用的。如下图所示，对象object 5、object 6、object 7虽然互相有关联,但是它们到GC Roots是不可达的,所以它们将会被判定为是可回收的对象。
        
        ![GC Root Set]({{ "/images" | prepend: site.baseurl }}//2017-07-09-java-basic-five-gc-root-set.png)

        > 在Java语言中,可作为GC Roots的对象包括下面几种:
        - 虚拟机栈(栈帧中的本地变量表)中引用的对象。
        - 方法区中类静态属性引用的对象。
        - 方法区中常量引用的对象。
        - 本地方法栈中JNI(即一般说的Native方法)引用的对象。
 


- GC 对性能会有哪些影响？
    + GC会stop-the-world，这意味着JVM因为需要执行GC而停止了应用程序的执行。当stop-the-world发生时，除GC所需的线程外，所有的线程都进入等待状态，直到GC任务完成。

- 如何通过 JVM 的参数调优 GC 的性能？
    + GC优化很多时候就是减少stop-the-world 的发生。
    + eden区空间不够存放新对象的时候，执行Minro GC。升到老年代的对象大于老年代剩余空间的时候执行Full GC，或者小于的时候被HandlePromotionFailure 参数强制Full GC。
    + 可以通过 NewRatio 控制新生代转老年代的比例，通过MaxTenuringThreshold 设置对象进入老年代的年龄阀值。

### 参考网址
* 1. [Java 新手的通病[5]：不了解 JVM](https://program-think.blogspot.com/2009/05/defect-of-java-beginner-5-jvm.html)
* 2. [GC是如何判断一个对象为"垃圾"的？被GC判断为"垃圾"的对象一定会被回收吗？ ](http://blog.csdn.net/canot/article/details/51037938)
* 3. [Java性能优化之JVM GC（垃圾回收机制）](https://zhuanlan.zhihu.com/p/25539690)

