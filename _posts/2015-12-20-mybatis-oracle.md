---
layout: post
title:  Oracle相关
date:   2015-12-20
categories: Java,WEB
comments: true
---

## What is the dual table in Oracle?##
- It's a sort of dummy table with a single record used for selecting when you're not actually interested in the data, but instead want the results of some system function in a select statement:
- e.g. select sysdate from dual;
- 这是一种单记录的虚拟表,在对具体查询数据不感兴趣,但对查询语句中的系统函数的返回值感兴趣的情况使用
- [What is the dual table in Oracle?](http://stackoverflow.com/questions/73751/what-is-the-dual-table-in-oracle)





## oracle sequence 在for循环中获取不连续 ##
要将12条记录在for循环中分别插入到orcle中.突发奇想,是不是可以用mybatis的foreache,这样做一次插入就可以了, 从而能够提高程序效率.先不管能不能提高效率, 这么做的时候出现了如下问题:

1. 程序是通过如下代码从sequence获取主键:

	```
		<select id="selectSequenceNext" resultType="java.math.BigDecimal">
		SELECT
		SEQ_XXX_ID.NEXTVAL
		FROM
		DUAL
		</select>
	```
2. for循环如下

	```java
	  for(Object obj : list){
	    logger.info(xxxMapper.selectSequenceNext());
	  }
	``` 
3. 结果发现得到的值都是同一个值.

原因推测:

1. 使用SQL语句在数据库查询的时候,sequence正常自增.故猜测很有可能是mybatis缓存的缘故, for循环查询速度太快了, 直接取的缓存.观察程序日志如下:

	```
	    2015-11-16 22:11:29.699 [admin] [http-apr-8080-exec-6] INFO  c.t.wt.w.service.impl.xxxServiceImpl - 28074
	    2015-11-16 22:11:32.806 [admin] [http-apr-8080-exec-6] DEBUG org.mybatis.spring.SqlSessionUtils - Fetched SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@42dbd127] from current transaction    
	    2015-11-16 22:11:32.806 [admin] [http-apr-8080-exec-6] DEBUG org.mybatis.spring.SqlSessionUtils - Releasing transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@42dbd127]
	```
从日志来看,加大了可能性.禁用缓存试试,[mybatis的缓存机制](http://blog.csdn.net/luanlouis/article/details/41408341)在得到sequence的select语句中加```useCache="false"```.重新跑代码,相同sequence依旧.好吧这个没效果,看来还得用上大招:**在sequence的select语句在加上```flushCache="true"```,非常棒,这下查出来的值都是不一样的了**.结果的select代码如下:

	```
	<select id="selectSequenceNext" resultType="java.math.BigDecimal" useCache="false" flushCache="true">
	    SELECT
	    SEQ_BACKUSER_ID.NEXTVAL
	    FROM
	    DUAL
	</select>
	```

###问题又来了,在解决问题的过程中,尝试过如下形式插入主键:###

	```
	<insert id="insertList" parameterType="java.util.List">
	    INSERT ALL
	
	    <foreach item="model" index="index" collection="list" open="" separator="" close="">
	        into TB_XXX
	        <trim prefix="(" suffix=")" suffixOverrides=",">
	            ID,
	            <if test="model.modifyTime != null">MODIFY_TIME,</if>
	        </trim>
	        <trim prefix="values (" suffix=")" suffixOverrides=",">
	            SEQ_XXX_ID.nextval,
	            <if test="model.modifyTime != null">#{model.modifyTime,jdbcType=DATE},</if>
	        </trim>
	    </foreach>
	    SELECT * FROM dual
	</insert>
	```
发现也会出现SEQ_XXX_ID.nextval得到的值是一样的情况.我估计是mybatis对代码进行了优化,实际操作中要避免此种方式操作.

### 最后说说效率###
这种用mybatis的foreach方法插入的方式并没有对实际效率提升多少, 充其量也就是形成了一个巨大的sql语句而已. 引用查资料时候看到的[一句话](http://qnalist.com/questions/5130976/mybatis-oracle-batch-update-using-foreach)
> We generally do not recommend this approach.  It's better to write a single
update statement and make the loop in Java.  If you are concerned about
performance, then you can use the batch executor.  This is not a batch, it
is just one giant statement.


这个还是推荐直接用java形式进行的for循环插入.起码这样不容易出错.
###参考链接###
1. [《深入理解mybatis原理》 MyBatis的二级缓存的设计原理](http://blog.csdn.net/luanlouis/article/details/41408341)
2. [Mybatis Oracle Batch Update Using Foreach](http://qnalist.com/questions/5130976/mybatis-oracle-batch-update-using-foreach)