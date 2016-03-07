# spring mvc 乱码问题 #

项目环境是在sping boot下, 在spring mvc下通过调用HttpServletResponse.write写文件返回时,发现写回来的文件乱码.而导出到本地不乱码.

```java
@RequestMapping("/list/excel")
public void excel( HttpServletResponse response) throws IOException {
	String test = "fff呵呵";
	response.getOutputStream().write(test.getBytes("iso-8859-1"));
}
```
返回fff??

改成utf-8 
```java
	@RequestMapping("/list/excel")
	public void excel( HttpServletResponse response) throws IOException {
		String test = "fff呵呵";
		response.getOutputStream().write(test.getBytes("utf-8"));
	}
```
返回fff鍛靛懙

-----
查询网上资料说加上htttp请求头就好了
```java
	@RequestMapping("/list/excel")
	public void excel( HttpServletResponse response) throws IOException {
		String test = "fff呵呵";
		response.setCharacterEncoding("UTF-8");
		response.getOutputStream().write(test.getBytes("utf-8"));
	}
```
	返回fff鍛靛懙
-----
加上response.setContentType("charset=UTF-8");
```java
	@RequestMapping("/list/excel")
	public void excel( HttpServletResponse response) throws IOException {
		String test = "fff呵呵";
		response.setContentType("charset=UTF-8");
		response.getOutputStream().write(test.getBytes("utf-8"));
	}
```
	返回fff鍛靛懙
---------
这才是正确的姿势,ContentType  不加上具体的类型,它不认你.
```java
	@RequestMapping("/list/excel")
	public void excel( HttpServletResponse response) throws IOException {
		String test = "fff呵呵";
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/json;charset=UTF-8");
		response.getOutputStream().write(test.getBytes("utf-8"));
	}
```
-------
现在我想导出一个poi生成的excel要怎么办
[java中使用poi导出Excel详解](http://gaochun091024.blog.51cto.com/6643038/1242195)
[stackoverflow上的一个问题](http://stackoverflow.com/questions/2937465/what-is-correct-content-type-for-excel-files)

观察了下,导出到本地io的文件是ok的,通过HttpServletResponse得到的流里面都是`锟斤拷`也就是说,已经被转码了


