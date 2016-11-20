---
layout: post
title:  "ElasticSearch java Demo"
date:   2016-11-20
categories: 工具

---
前段时间研究了下Elasticsearch，今天抽空整理份elsticsearch最简单的java api 调用demo出来。
首先去[官网](https://www.elastic.co/products/elasticsearch "官网")下好ElasticSearch的运行程序。根据安装要求安装，我这里就不一一赘述了。
安装好后，新建maven工程，引入包：

```xml
<dependency>
    <groupId>org.elasticsearch</groupId>
    <artifactId>elasticsearch</artifactId>
    <version>2.4.1</version>
</dependency>
```

我的代码是基2.4.1版本的。

不废话，看CRUD代码，先是Bean类

```java
public class Blog {
    private Integer id;
    private String title;
    private String posttime;
    private String content;

    public Blog() {
    }

    public Blog(Integer id, String title, String posttime, String content) {
        this.id = id;
        this.title = title;
        this.posttime = posttime;
        this.content = content;
    }
    //setter and getter
}
```


然后是数据工厂类：

```java
package com.fengzii;

import com.alibaba.fastjson.JSON;

import java.util.ArrayList;
import java.util.List;

public class DataFactory {
    public static DataFactory dataFactory = new DataFactory();

    private DataFactory() {
    }

    public DataFactory getInstance() {
        return dataFactory;
    }

    public static List<String> getInitJsonData() {
        List<String> list = new ArrayList<String>();
        String data1 = JSON.toJSONString(new Blog(1, "csdn git简介", "2016-06-19", "Git是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。 "));
        String data2 = JSON.toJSONString(new Blog(2, "csdn Java中泛型的介绍与简单使用", "2016-06-19", " git现在开始深入学习Java的泛型了，以前一直只是在集合中简单的使用泛型，根本就不明白泛型的原理和作用。"));
        String data3 = JSON.toJSONString(new Blog(3, "csdn SQL基本操作", "2016-06-19", "日期函数  "));
        String data4 = JSON.toJSONString(new Blog(4, "csdn Hibernate框架基础", "2016-06-19","hibernate是一个基于jdbc的开源的持久化框架，是一个优秀的ORM实现，它很大程度的简化了dao层编码工作。Hibernate对JDBC访问数据库的代码做了封装，"));
        String data5 = JSON.toJSONString(new Blog(5, "csdn Shell基本知识", "2016-06-19", "日常的linux系统管理工作中必不可少的就是shell脚本，如果不会写shell脚本，那么你就不算一个合格的管理员。"));
        String data6 = JSON.toJSONString(new Blog(6, "git", "2016-06-19", "git"));
        list.add(data1);
        list.add(data2);
        list.add(data3);
        list.add(data4);
        list.add(data5);
        list.add(data6);
        return list;
    }

}
```

主要的java操作类，在这里面，简单的实现了CRUD的操作：

```java
package com.fengzii;

import com.alibaba.fastjson.JSON;
import org.elasticsearch.action.delete.DeleteResponse;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.action.search.SearchType;
import org.elasticsearch.action.update.UpdateRequest;
import org.elasticsearch.client.Client;
import org.elasticsearch.client.Requests;
import org.elasticsearch.client.transport.TransportClient;
import org.elasticsearch.common.transport.InetSocketTransportAddress;
import org.elasticsearch.index.query.QueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHits;

import java.io.IOException;
import java.net.InetAddress;
import java.net.UnknownHostException;
import java.util.List;
import java.util.concurrent.ExecutionException;

import static org.elasticsearch.common.xcontent.XContentFactory.jsonBuilder;

public class ElasticSearchHandler {
    private static Client client;

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        try {
            /* 创建客户端 */
            client = TransportClient.builder().build()
                    .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9300));

            //插入数据
            saveToEs();

            // Prepare and execute query
            SearchResponse resp = searchEs();

            SearchHits shs = resp.getHits();

            //update
            updateEs(shs);

            resp = searchEs();
            //delete
            deleteEs(shs);
            resp = searchEs();


        } catch (UnknownHostException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            client.close();
        }

    }

    /*
        删除传进来的SearchHits里所有的记录
     */
    private static void deleteEs(SearchHits shs) {
        for (int i = 0; i < shs.getTotalHits(); i++) {

            DeleteResponse response = client.prepareDelete("blog", "article1", shs.getAt(i).getId()).get();
            System.out.println(JSON.toJSONString(response));
        }
    }

    /*
    更新传进来的shs中的第一条记录，设置title为1git简介1111
     */
    private static void updateEs(SearchHits shs) throws IOException, InterruptedException, ExecutionException {
        UpdateRequest updateRequest = new UpdateRequest();
        updateRequest.index(shs.getAt(0).getIndex());
        updateRequest.type(shs.getAt(0).getType());
        updateRequest.id(shs.getAt(0).getId());
        updateRequest.doc(jsonBuilder()
                .startObject()
                .field("title", "1git简介1111")
                .endObject());
        client.update(updateRequest).get();
    }

    /*
    查询title或content中带有git的记录
     */
    private static SearchResponse searchEs() {
        // Refresh index reader
        client.admin().indices().refresh(Requests.refreshRequest("_all")).actionGet();
        QueryBuilder queryBuilder = QueryBuilders.termQuery("title", "git");
        QueryBuilder queryBuilder1 = QueryBuilders.termQuery("content", "git");
        SearchResponse resp = client.prepareSearch("blog")
                .setTypes("article1")
                .setSearchType(SearchType.DEFAULT)
                .setQuery(queryBuilder).setQuery(queryBuilder1)
//                .setFrom(0).setSize(60).setExplain(true)
                .execute()
                .actionGet();
        System.out.println(JSON.toJSONString(resp.getHits()));
        return resp;
    }

    /*
    存储数据进es
     */
    private static void saveToEs() {
        List<String> jsonData = DataFactory.getInitJsonData();

        for (int i = 0; i < jsonData.size(); i++) {
            IndexResponse response = client.prepareIndex("blog", "article1").setSource(jsonData.get(i)).get();
            if (response.isCreated()) {
                System.out.println("创建成功!23232");
            }
        }
    }
}
```


## 参考##
1. [很好的一个中文文档](http://es.xiaoleilu.com/ "很好的一个中文文档")
2. [ElasticSearch是什么](http://baike.baidu.com/link?url=jCYM5_MSVWT5IaknBWzxiWU7nILm-cOi6Uqz-V6PSuTtLB-pFhXRzQbLZNV0c20NEue1QFfHQFhQgpk57gawJ8SJ9zEAbcdizTzEU777D57 "ElasticSearch是什么")
3. [官方文档](https://www.elastic.co/guide/index.html)
