---
layout: post
title:  "Java学习之String，StringBuffer，StringBuilder"
date:   2015-4-24 14:40:35
categories: Java基础
---
### 一、String,StringBuffer, StringBuilder 的区别是什么？String为什么是不可变的？###
1. String是常量，它们的值在创建之后不能更改。（常量的定义：）
2. 因为是常量，对String的+操作的重载存在问题。虽然通过引入StringBuilder优化过String的+，但是在循环中使用时，还是存在性能问题。
* 循环中StringBuilder的对象创建个数。（使用javap -c 查看[eclipse中使用javap](http://stackoverflow.com/questions/7056987/how-to-use-javap-with-eclipse) ）    
 ```java 
		public class StringTest {
		  public static void main(String[] args) {
		    String mango = "mango";
		    String s = "abc"+ mango+ "def"+47;
		    System.out.println(s);
		  }
		} 
 ```
如上Java代码用`javap -c Concatenation` 解析出来如下： 
```c
Compiled from "StringTest.java"
public class test.StringTest {
public test.StringTest();
Code:
0: aload_0       
1: invokespecial #8                  // Method java/lang/Object."<init>":()V
4: return        

public static void main(java.lang.String[]);
Code:
0: ldc           #16                 // String mango
2: astore_1      
3: new           #18                 // class java/lang/StringBuilder
6: dup           
7: ldc           #20                 // String abc
9: invokespecial #22                 // Method java/lang/StringBuilder."<init>":(Ljava/lang/String;)V
12: aload_1       
13: invokevirtual #25                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
16: ldc           #29                 // String def
18: invokevirtual #25                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
21: bipush        47
23: invokevirtual #31                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
26: invokevirtual #34                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
29: astore_2      
30: getstatic     #38                 // Field java/lang/System.out:Ljava/io/PrintStream;
33: aload_2       
34: invokevirtual #44                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
37: return        
}
```
我们可以发现Java编译器的处理是，新建StringBuilder对象，使用该对象完成对字符串的+操作后，最终将StringBuilder转换成String返回。这一定程度上提高了，程序的处理效率。
*  但是这并不意味着我们可以随意使用String对象，反正编译器可以为我们优化性能。从以下代码可以看出来：
```java
public class StringTest {
  public String implicit(String[] fields){
    String result = "";
    for(int i=0; i<fields.length; i++){
      result += fields[i];
    }
    return result;
  }
  
  public String explicit(String [] fields){
    StringBuilder result = new StringBuilder();
    for(int i=0; i<fields.length; i++){
      result.append(fields[i]);
    }
    return result.toString();
  }

}
```
对其进行编译处理
```c
Compiled from "StringTest.java"
public class test.StringTest {
  public test.StringTest();
    Code:
       0: aload_0       
       1: invokespecial #8                  // Method java/lang/Object."<init>":()V
       4: return        

  public java.lang.String implicit(java.lang.String[]);
    Code:
       0: ldc           #16                 // String 
       2: astore_2      
       3: iconst_0      
       4: istore_3      
       5: goto          32
       8: new           #18                 // class java/lang/StringBuilder
      11: dup           
      12: aload_2       
      13: invokestatic  #20                 // Method java/lang/String.valueOf:(Ljava/lang/Object;)Ljava/lang/String;
      16: invokespecial #26                 // Method java/lang/StringBuilder."<init>":(Ljava/lang/String;)V
      19: aload_1       
      20: iload_3       
      21: aaload        
      22: invokevirtual #29                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      25: invokevirtual #33                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      28: astore_2      
      29: iinc          3, 1
      32: iload_3       
      33: aload_1       
      34: arraylength   
      35: if_icmplt     8
      38: aload_2       
      39: areturn       

  public java.lang.String explicit(java.lang.String[]);
    Code:
       0: new           #18                 // class java/lang/StringBuilder
       3: dup           
       4: invokespecial #45                 // Method java/lang/StringBuilder."<init>":()V
       7: astore_2      
       8: iconst_0      
       9: istore_3      
      10: goto          24
      13: aload_2       
      14: aload_1       
      15: iload_3       
      16: aaload        
      17: invokevirtual #29                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      20: pop           
      21: iinc          3, 1
      24: iload_3       
      25: aload_1       
      26: arraylength   
      27: if_icmplt     13
      30: aload_2       
      31: invokevirtual #33                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      34: areturn       
}

```
直接使用StringBuilder和使用String然后依靠编译器优化的区别为，一个是由编译器在循环内自动生成的StringBuilder，一个是在循环外手动创建的StringBuilder。很明显，在编译器自动优化时会产生大量的多余的StringBuilder对象。因此在循环中使用toString等操作时，最好自己创建一个StringBuilder对象。

* StringBuilder的常用的方法有
	* append()
	* toString()
	* delete(int start包含, int end不包含) 


3. StringBuilder和StringBuffer的区别:

StringBuffer是线程安全的，而StringBuilder是非线程安全的。ps：线程安全会带来额外的系统开销，所以StringBuilder的效率比StringBuffer高。如果对系统中的线程是否安全很掌握，可用StringBuilder，在线程不安全处加上关键字Synchronize。

























