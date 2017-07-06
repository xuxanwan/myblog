---
layout: post
title:  "Java新手问题 02 面向对象基本功"
date:   2017-07-02
categories: Java
comments: true
---

### Question: 基于接口的继承和基于实现的继承各有什么优缺点

#### 接口的本质: 

- 协议,约定,能力. 接口这个概念在生活中并不陌生，电子世界中一个常见的接口就是USB接口。电脑往往有多个USB接口，可以插各种USB设备，可以是键盘、鼠标、U盘、摄像头、手机等等。接口声明了一组能力，但它自己并没有实现这个能力，它只是一个约定，它涉及交互两方对象，一方需要实现这个接口，另一方使用这个接口，但双方对象并不直接互相依赖，它们只是通过-接口间接交互
 
#### 实现的本质:
- 事物本身要做的具体功能,具体操作

#### 继承的本质:
- 对具体功能的扩展,泛化
- 之所以叫继承是因为，子类继承了父类的属性和行为，父类有的属性和行为，子类都有。但子类可以增加子类特有的属性和行为，某些父类有的行为，子类的实现方式可能与父类也不完全一样。使用继承一方面可以复用代码，公共的属性和行为可以放到父-类中，而子类只需要关注子类特有的就可以了，另一方面，不同子类的对象可以更为方便的被统一处理。

#### 基于接口的继承:
- 优点:
	- 扩展灵活,代表协议层面的扩充. 可以在原接口上提供能力的扩展. 
	- 接口的继承在降低耦合度方面有很好的作用,接口的继承不像实现的继承那样破坏封装.
- 缺点:
	- 只是针对接口能力的扩展,   没有具体实现代码的复用. 具体可复用实现需要具体处理.

#### 基于实现的继承:
- 对原有实现的扩展,可以是父类的不同实现子类

- 优点:
	- 子类可以复用父类代码，不写任何代码即可具备父类的属性和功能，而只需要增加特有的属性和行为。
	- 子类可以重写父类行为，还可以通过多态实现统一处理。
	- 给父类增加属性和行为，就可以自动给所有子类增加属性和行为。
	- 另一个是利用多态和动态绑定统一处理多种不同子类的对象
- 缺点:
	- 破坏类封装,子类的的代码和父类耦合，如果子类不知道基类方法的实现细节，它就不能正确的进行扩展。
	- 降低代码灵活性,子类必须要有父类的的所有public方法,子类不再自由.
	- 侵入性,必须要有父类的所有方法
	
### Question: 继承（包括 extend 和 implement）有什么【缺点】
-  extends 是继承一个具体实体类
-  implements  是继承一个接口.
	- 缺点: 
		- 子类会依赖于父类存在无法解耦,父类的更改会影响子类
			- 父类新增方法
			- 父类删除方法
			- 子类脱离父类无法独立存在
		- 性能上会有影响,如果有十层继承,那么执行一个方法需要寻找十层
	- 优点:
		- 增加代码复用,重复的代码可以在父类中定义,子类可以很便利的调用
		- 覆盖,子类可以重写父类的实现
		- 扩展性,子类可以增加自己的实现
		- 接口会使得程序变得更加灵活
		- 隐藏数据,父类可以定义一些属性为private,这样子类就需要重定义

### Question: 多态（polymorphism）有什么【缺点】？
- 性能问题（这个比较轻微）；
- 比如在“实现继承”中的多态，可能会附带有继承的缺点。

### Question: 为什么 Java 可以多继承 interface，而不可以多继承 class？
- 菱形继承问题,若果a,b都有一个add方法.c多继承a,b; 那么c无法确定该用哪个父类的add方法.除了方法,变量也有这类问题.

### Question: 假如让你写一个小游戏（比如人机对战的五子棋），你会如何设计类结构？
- 棋盘(放置棋子接口/获取当前棋盘情况接口/和棋计算) 
	- 虚拟棋盘
		- 棋位置的二维数组
- 棋子
	- 白棋子
	- 黑棋子
- 棋手(下棋接口)
	- 电脑
	- 人类
	
### Question: 类结构设计时，如何考虑可扩展性？
我认为做到如下几点就可以保证扩展性了.
- 单一职责原则:
	Single Responsibility Principle
- 里氏替换原则:
	If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T,the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T.（如果对每一个类型为S的对象o1，都有类型为T的对象o2，使得以T定义的所有程序P在所有的对象o1都代换成o2时，程序P的行为没有发生变化，那么类型S是类型T的子类型。）
- 依赖倒置原则:
	Dependence Inversion Principle.High level modules should not depend upon low level modules.Both should depend upon abstractions.Abstractions should not depend upon details.Details should depend upon abstractions.
- 接口隔离原则:
	Clients should not be forced to depend upon interfaces that they don't use; The dependency of one class to another one should depend on the smallest possible interface.
- 迪米特法则:
	Least Knowledge Principle
- 开闭原则: 
	Software entities like classes, modules and functions should be open for extension but closed for modifications.
	
### 参考网址
* 1.[Inheritance Advantages and Disadvantages](https://erpbasic.blogspot.jp/2012/01/inheritance-advantages-and.html)
* 2.[老马说编程](https://www.cnblogs.com/swiftma/p/5587851.html)

