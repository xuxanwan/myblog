---
layout: post
title:  "Java之Collection"
date:   2015-4-29 09:16:03
categories: Java基础

---
### 一、Collection

+ List 按照插入的顺序保存元素
	- 若使用的都为List接口包含的方法，在对象的创建处能够方便的修改实现，例如:
	`List<Apple> apples = new ArrayList<Apple>();`
	`List<Apple> apples = new ArrayList<Apple>();`
+ Set 不能有重复元素
	- HashSet 最快的获取元素的存储方式。
	- TreeSet 按照比较结果升序的保存对象。
	- LinkedHashSet 按照被添加的顺序存储元素。 
+ Queue	通过排队规则确定对象的顺序
+ Map 将某些对象和另外一些对象关联在一起
	- HashMap 提供了最快的查找技术，无序。
	- TreeMap  按照比较结果升序保存键。
	- LinkedHashMap 按照插入顺序保存键，同时保存了HashMap的查询速度。


### 二、List的一些工具方法
- `System.arraycopy(src, srcPos, dest, destPos, length);` 待拷贝，起始位置， 目标地址，起始位置，长度
- `Arrays.asList(1, 2, 3, 4, 5);`接受用逗号分隔的元素列表装换成List
- 一组元素的增加
	+  `Collection<Integer> collection = new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4, 5));`
	+  `collection.addAll(Arrays.asList(moreInts));`
	+  `Collections.addAll(collection, 11, 12, 13, 14, 15);`
	+  `Collections.addAll(collection, moreInts);`
	+  注意事项:`List<Integer> list = Arrays.asList(16, 17, 18, 19, 20);`这样初始化的list本质上还是数组,故不能进行List集合的相关操作.
- 可以通过Collections.synchronizedList(list)对ArrayList进行加锁操作。
- 对通过.subList()方法获取的的子集的修改会影响到原有的List



### 三、Vector,ArrayList,LinkedList的区别

- ArrayList 和Vector都是采用数组方式存储数据，数组元素数大于实际存储的数据个数以便增加和插入元素。
	+ 优点：两者都允许直接序号索引元素，较为方便。
	+ 缺点：插入数据要涉及到数组元素移动等内存操作，所以索引数据快插入数据慢。
- Vector由于使用了**synchronized**，所以性能上比ArrayList要差，但适用于多线程情况下。但 Vector在缺省情况下自动增长原来一倍的数组长度，ArrayList是原来的50%,所以最后获得的集合所占的空间比实际需要的大。所以如果要在集合中保存大量的数据那么使用Vector有一些优势，因为你可以通过设置集合的初始化大小来避免不必要的资源开销。
- LinkedList使用双向链表实现存储，按序号索引数据需要进行向前或向后遍历，但是插入数据时只需要记录本项的前后项即可
	+ 所以插入数度较快
	+ 索引数据速度慢

### 四、使用
你只是查找特定位置的元素或只在集合的末端增加、移除元素，那么使用Vector或ArrayList都可以。如果是其他操作，你最好选择其他的集合操作类。比如，LinkList集合类在增加或移除集合中任何位置的元素所花费的时间都是一样的?O(1)，但它在索引一个元素的使用却比较慢－O(i),其中i是索引的位置.使用ArrayList也很容易，因为你可以简单的使用索引来代替创建iterator对象的操作。LinkList也会为每个插入的元素创建对象，所有你要明白它也会带来额外的开销。
最后，在《Practical Java》一书中Peter Haggar建议使用一个简单的数组（Array）来代替Vector或ArrayList。尤其是对于执行效率要求高的程序更应如此。因为使用数组(Array)避免了同步、额外的方法调用和不必要的重新分配空间的操作。



### 五、HashTable, HashMap，TreeMap，LinkedHashMap的区别

* HashMap：根据hash表实现，存储方式是无序的。
* HashTable：相对于HashMap，这个是同步的。
* TreeMap：根据红黑树实现，他是更具key的顺序排序的。
* LinkedHashMap：保存了插入的顺序。

### 六、Map的使用
+ HashMap的遍历方式


### 七、Set的区别

### 八、Set的使用

### 九、Queue的区别

### 十、Queue的使用

