---
layout: post
title:  "Java之Collection一"
date:   2015-4-29 09:16:03
categories: Java基础

---
## Collection
Collection大致可以分为List,Set,Queue,Map四个种类.

+ List 按照插入的顺序保存元素
	- 若使用的都为List接口包含的方法，在对象的创建处能够方便的修改实现，例如:
{% highlight java %}
List<Apple> apples = new ArrayList<Apple>();
apples = new LinkedList<Apple>();
{% endhighlight %}
+ Map 将某些对象和另外一些对象关联在一起
	- HashMap 提供了最快的查找技术，无序。
	- TreeMap  按照比较结果升序保存键。
	- LinkedHashMap 按照插入顺序保存键，同时保存了HashMap的查询速度。缺点这是是最最占空间的Map
+ Set 不能有重复元素,Set其实是Map留下相应的key,将value指向同一个object的结果.相同的
	- HashSet 最快的获取元素的存储方式。
	- TreeSet 按照比较结果升序的保存对象。
	- LinkedHashSet 按照被添加的顺序存储元素。 
+ Queue	通过排队规则确定对象的顺序

这次来学习归纳Map.

##二、Map

#### HashMap：

* 以Entry[]数组实现,根据key的hash值取模Entry[].length得到数组下标(hash(key)%len).最终存储方式是无序的。插入元素时，如果两条Key落在同一个数组中(比如哈希值1和17取模16后都属于第一个Entry)，Entry用一个next属性实现多个Entry以单向链表存放，先入Entry将next指向桶当前的Entry。

![HashMap.png]({{ "/public/upload" | prepend: site.baseurl }}/HashMap.png)

* 当Entry[]使用了75%时,会成倍扩容Entry[]，并重新分配所有原来的Entry，常用的优化是在初始化时对HashMap的大小有个预估值.


#### HashTable：

* Hashtable 也是一个散列表，它存储的内容是键值对(key-value)映射。相对于HashMap它是同步的。它的key、value都不可以为null。Hashtable中的映射不是有序的。

#### TreeMap：
* TreeMap 是一个有序的key-value集合，它是通过红黑树实现的。实现了NavigableMap接口意味着它支持一系列的导航方法。比如返回有序的key集合。树上插入/删除元素的代价比HashMap的大。

#### LinkedHashMap：
* 在散列化保存数据的同时

### 2、Map的使用
+ HashMap的遍历方式


### 七、Set的区别

### 八、Set的使用

### 九、Queue的区别

### 十、Queue的使用



##参考链接

1. [江南白衣~关于Java集合的小抄](http://calvin1978.blogcn.com/articles/collection.html)
2. [Java 集合系列12之 TreeMap详细介绍(源码解析)和使用示例](http://www.cnblogs.com/skywang12345/p/3310928.html)
