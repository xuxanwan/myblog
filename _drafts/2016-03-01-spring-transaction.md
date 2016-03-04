spring boot 乱码问题


	@RequestMapping("/list/excel")
	public void excel( HttpServletResponse response) throws IOException {
		String test = "fff呵呵";
		response.getOutputStream().write(test.getBytes("iso-8859-1"));
	}

返回fff??

改成utf-8 
	@RequestMapping("/list/excel")
	public void excel( HttpServletResponse response) throws IOException {
		String test = "fff呵呵";
		response.getOutputStream().write(test.getBytes("utf-8"));
	}
返回fff鍛靛懙

-----
查询网上资料说加上htttp请求头就好了
	@RequestMapping("/list/excel")
	public void excel( HttpServletResponse response) throws IOException {
		String test = "fff呵呵";
		response.setCharacterEncoding("UTF-8");
		response.getOutputStream().write(test.getBytes("utf-8"));
	}

	返回fff鍛靛懙
-----
加上response.setContentType("charset=UTF-8");
	@RequestMapping("/list/excel")
	public void excel( HttpServletResponse response) throws IOException {
		String test = "fff呵呵";
		response.setContentType("charset=UTF-8");
		response.getOutputStream().write(test.getBytes("utf-8"));
	}

	返回fff鍛靛懙
---------
这才是正确的姿势,ContentType  不加上具体的类型,它不认你.
	@RequestMapping("/list/excel")
	public void excel( HttpServletResponse response) throws IOException {
		String test = "fff呵呵";
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/json;charset=UTF-8");
		response.getOutputStream().write(test.getBytes("utf-8"));
	}

-------
现在我想导出一个poi生成的excel要怎么办
[java中使用poi导出Excel详解](http://gaochun091024.blog.51cto.com/6643038/1242195)


