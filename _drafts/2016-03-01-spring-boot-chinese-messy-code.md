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

更奇葩的是,通过非springboot项目导出的文件是正常的. 难道springboot 对文件编码的时候瞎搞了?


-------
[设置了下CharacterEncodingFilter](http://stackoverflow.com/questions/24054648/how-to-config-characterencodingfilter-in-springboot)
下载文件还是乱码

```
@RequestMapping("/list/excel")
    public void excel(@RequestParam(value = "rcId") String rcId, @RequestParam(value = "startTime") String startTime,
                      @RequestParam(value = "endTime") String endTime, HttpServletResponse response) throws IOException {
        logger.info(startTime);
        logger.info(endTime);
        int id = Integer.parseInt(rcId);
        Date date1 = new Date();
        Date date2 = new Date(date1.getTime() - 24 * 3600);
    String fileName = "百日哦你个发你.xls";
    fileName = new String(fileName.getBytes(),"ISO8859-1");
    response.setCharacterEncoding("ISO8859-1");
		response.setContentType("application/vnd.ms-excel;charset=ISO8859-1");
//        response.setContentType("application/zip");
        response.setHeader("cache-control", "no-cache");
        response.setHeader("Content-Disposition", "attachment; filename="+fileName);

        List<Map<String, Object>> list = findFansRows(id, date1, date2);
        // 声明一个工作薄
        HSSFWorkbook workbook = new HSSFWorkbook();
        // 获取标题行
        String str = "";
        Map<String, Object> map = list.get(0);
        Set<String> keySet = map.keySet();
        for (String key : keySet) {
            str = str + key + ",";
        }
        String[] headers = str.split(",");
        System.out.println("标题为：" + Arrays.toString(headers));
        ExcelExport<Map<String, Object>> export = new ExcelExport<Map<String, Object>>();
        OutputStream out =  response.getOutputStream();

        try {
            OutputStream fileOutputStream = new FileOutputStream("D://a.zip");
            ZipOutputStream zipOutputStream = new ZipOutputStream(fileOutputStream);
            ZipEntry ze = new ZipEntry("spy.xls");
            zipOutputStream.putNextEntry(ze);




            export.mapExport(workbook, "测试", headers, list);
			workbook.write(out);
            workbook.write(zipOutputStream);

            zipOutputStream.close();


            FileInputStream fileInputStream = new FileInputStream("D://a.zip");

            byte[] buf = new byte[8194];
            int len;
            while ((len = fileInputStream.read(buf)) != -1) {
                out.write(buf, 0, len);
                System.out.println(JSON.toJSONString(buf));
            }


            out.flush();
//            fileInputStream.close();
            out.close();

        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }

    }

    ```
    ----------------
    我的心好累,还是解决不掉
    [downloading-a-file-from-spring-controllers](http://stackoverflow.com/questions/5673260/downloading-a-file-from-spring-controllers)