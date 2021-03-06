###javascript 内建函数

####JavaScript splice() 方法

- splice() 方法用于插入、删除或替换数组的元素。
注意：这种方法会改变原始数组！。


- returnArray array.splice(index,howmany,item1,.....,itemX)



|参数|描述|
|----|------|
|index|必需。规定从何处添加/删除元素。该参数是开始插入和（或）删除的数组元素的下标，必须是数字。|
|howmany|必需。规定应该删除多少元素。必须是数字，但可以是 "0"。如果未规定此参数，则删除从 index 开始到原数组结尾的所有元素。|
|item1, ..., itemX|可选。要添加到数组的新元素|
|returnArray|如果从 arrayObject 中删除了元素，则返回的是含有被删除的元素的数组。|


- 示例
```javascript
var aa = ['aa','bb','cc','dd'];
aa.splice(0,1);
console.info('删除0下标下一个元素:'+aa);
aa.splice(0,0,'ee');
console.info('不删除,直接往下标增加ee:'+aa);
```

--------------
###javascript 闭包

####参考
[学习Javascript闭包（Closure）](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)
[理解Javascript的闭包](http://coolshell.cn/articles/6731.html)
[JavaScript 里的闭包是什么？应用场景有哪些？](https://www.zhihu.com/question/19554716)
####理解
- 闭包是什么:
各种专业文献上的"闭包"（closure）定义非常抽象，很难看懂。我的理解是，闭包就是能够读取其他函数内部变量的函数。
由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。
所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。
- 为什么要闭包
变量的作用域无非就是两种：全局变量和局部变量。
Javascript语言的特殊之处，就在于函数内部可以直接读取全局变量。
另一方面，在函数外部自然无法读取函数内的局部变量。
出于种种原因，我们有时候需要得到函数内的局部变量。但是，前面已经说过了，正常情况下，这是办不到的，只有通过变通方法才能实现。
- 闭包怎么实现
```javascript
var name = "The Window";
var object  = {
	name:"object name",
	aaFunc:function(){
		var name = 'hello world';
		var that = this;
		return function(){
			return name//return that.name 返回方法体内的name
		}
	}	
} 
alert(object.aaFunc()());
```
