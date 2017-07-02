##spring mvc 传值方式
###从前段页面接受参数方式:
- 从前端接受一个数组参数:
通常在前端传递过来一个同样名字的参数时候使用`phone=val1&phone=val2&phone=val3`


```java
	public String method(@RequestParam(value="phone") String[] phoneArray){
	    ....
	}

```

然后我们可以通过`Arrays.asList(..)`来转换


###参考
- [数组传值](http://stackoverflow.com/questions/9768509/can-spring-mvc-handle-multivalue-query-parameter)