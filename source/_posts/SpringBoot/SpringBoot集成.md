---
title: SpringBoot集成Solr
date: 2021-10-13
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1633779120000.webp
tags: SpringBoot集成
categories: SpringBoot
---
## 前言

本章使用SpringBoot继承Solr8.10.0简单的操作一下solr

## 前置条件

- solr 开启状态中
- 有一个core

##  整合solr

### 引入依赖

```xml
        <!--添加 整合solr的依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-solr</artifactId>
            <version>2.4.11</version>
        </dependency>
```

### 配置solr服务器地址

```yml\
spring:
  data:
    solr:
      host: http://192.168.128.214:8983/solr/hotel_core
```

### 使用Solr提供的客户端操作API

> @Resource
>
> private SolrClient solrClient;

### 实现添加及修改操作

```test
@Test
    void createDocument() throws IOException, SolrServerException {
        //创建索引对象
        SolrInputDocument document =new SolrInputDocument();
        document.setField("id","55");
        document.setField("userCode","2734@qq.com");

        //添加索引
        solrClient.add(document);
        solrClient.commit();
        System.out.println("添加索引库成功!");

    }
```

### 实现查询操作

```test
@Test
void queryDocument() throws IOException, SolrServerException {
    SolrQuery query = new SolrQuery();
    //查询条件:默认查询所有
    query.setQuery("*:*");
    query.setSort("id",SolrQuery.ORDER.desc);

    //查询结果的封装对象
    QueryResponse queryResponse = solrClient.query(query);
    //查询结果
    SolrDocumentList results = queryResponse.getResults();
    //查询到的数量
    long numFound = results.getNumFound();
    System.out.println("总共有几条--->"+numFound);
    //遍历查询结果
    for (SolrDocument result :results) {
        System.out.println("id-->"+result.get("id"));
        System.out.println("userCode-->"+result.get("userCode"));
    }
}
```

### 实现删除操作

- 根据id删除

  ```text
  @Test
  void delById() throws IOException, SolrServerException {
      solrClient.deleteById("55");
      solrClient.commit();
  }
  ```

- 根据key和value删除

  ```xml
  
  @Test
  void delByQuery() throws IOException, SolrServerException {
      solrClient.deleteByQuery("id:55");
      solrClient.commit();
  }
  ```

  



