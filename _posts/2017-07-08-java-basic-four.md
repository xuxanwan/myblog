---
layout: post
title:  "Java新手问题 04 虚拟机相关"
date:   2017-07-08
categories: Java
comments: true
---

### 关于基本类型和引用类型
    主要是关于:基本类型和引用类型在本质上有什么区别. 
    基本类型主要包括:
    boolean、byte、short、char、int、long、float、double。
    其它所有的类型都属于引用类型。


#### Question:这两种类型在内存存储上有什么区别
+ 基础类型在声明的同时系统会给予分配内存空间.
+ 引用类型在声明时系统只是分配了引用空间,数据空间没有分配.引用类型变量在声明后必须通过实例化开辟数据空间，才能对变量所指向的对象进行访问。
+ ![内存中两种类型的状态]({{ "/images" | prepend: site.baseurl }}//2017-07-08-java-basic-four-mem-of-primary-ref.png)
+ 如上图所示,基础类型放在栈内存(Stack memory)中,而引用类型是将引用地址放在栈内存中,实际对象放在堆内存(Heap memory)中。

#### Question:这两种类型在性能上有什么区别
这要从堆栈的优势和区别说起：
+ 栈的优势是，存取速度比堆要快，仅次于寄存器，栈数据**可以共享**。但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。
+ 而堆是一个运行时数据区,类的(对象从中分配空间。这些对象通过new、newarray、anewarray和multianewarray等指令建立，它们不需要程序代码来显式的释放。堆是由垃圾回收来负责的，堆的优势是可以动态地分配内存大小，生存期也不必事先告诉编译器，因为它是在运行时动态分配内存的，Java的垃圾收集器会自动收走这些不再使用的数据。但缺点是，由于要在运行时动态分配内存，存取速度较慢。
+ 所以性能方面来说，使用基础数据类型肯定会比使用引用数据类型速度快。
> 堆相对进程来说是全局的，能够被所有线程访问；而栈是线程局部的，只能本线程访问。打个比方，栈就好比个人小金库，堆就好比国库。你从个人小金库拿钱去花，不需要办什么手续，拿了就花，但是钱数有限；而国库里面的钱虽然很多，但是每次申请花钱要打报告、盖图章、办 N 多手续，耗时又费力。
　　同样道理，由于堆是所有线程共有的，从堆里面申请内存要进行相关的加锁操作，因此申请堆内存的复杂度和时间开销比栈要大很多；从栈里面申请内存，虽然又简单又快，但是栈的大小有限，分配不了太多内存。

#### Question:这两种类型对于 GC有什么区别
+ 基本类型的数据是可以共享的,所以整个jvm可能会只有一个基础类型,回收相对较快也较容易.

### 参考网址
* 1. [Java 新手的通病[5]：不了解 JVM](https://program-think.blogspot.com/2009/05/defect-of-java-beginner-5-jvm.html)
* 2. [What's the difference between primitive and reference types?](https://stackoverflow.com/questions/8790809/whats-the-difference-between-primitive-and-reference-types)

