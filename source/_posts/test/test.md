---
title: Test f
categories: program
tags: elasticsearch
comments: true
date: 2018-12-14
---

## Elasticsearch

### Foreword



### Why ES

- 成熟
- 高可用
- 高扩展


### Development Environment

- elasticsearch

  ```bash
  ## version
  6.6.0
  ```

- kibana

  ```bash
  ## version
  6.6.0
  ```

### Point Conception

- [Apache Lucene](https://lucene.apache.org/)

  Lucene是一套用于全文检索和搜寻的开放源码程式库，由Apache软件基金会支持和提供。Lucene提供了一个简单却强大的应用程序界面，能够做全文索引和搜寻，在Java开发环境里Lucene是一个成熟的免费开放源代码工具；就其本身而论，Lucene是现在并且是这几年，最受欢迎的免费Java资讯检索程式库。

- Inverted Index

  Lucene实现快速搜索的核心就是倒排索引

- [全文搜索引擎](https://baike.baidu.com/item/全文搜索引擎)

  全文[搜索引擎](https://baike.baidu.com/item/%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E/104812)是目前广泛应用的搜索引擎。它的工作原理是计算机[索引](https://baike.baidu.com/item/%E7%B4%A2%E5%BC%95)程序通过扫描文章中的每一个词，对每一个词建立一个[索引](https://baike.baidu.com/item/%E7%B4%A2%E5%BC%95/5716853)，指明该词在文章中出现的次数和位置，当用户查询时，检索程序就根据事先建立的索引进行查找，并将查找的结果反馈给用户的检索方式。这个过程类似于通过字典中的检索字表查字的过程。

- Java

  Elasticsearch是使用 Java 开发的

- [Solr](https://lucene.apache.org/solr/)

  Solr是用[Java](https://zh.wikipedia.org/wiki/Java)编写、运行在[Servlet](https://zh.wikipedia.org/wiki/Servlet)容器（如[Apache Tomcat](https://zh.wikipedia.org/wiki/Apache_Tomcat)或[Jetty](https://zh.wikipedia.org/wiki/Jetty)）的一个独立的全文搜索服务器。 Solr采用了[Lucene](https://zh.wikipedia.org/wiki/Lucene) Java搜索库为核心的全文索引和搜索，并具有类似[REST](https://zh.wikipedia.org/wiki/REST)的[HTTP](https://zh.wikipedia.org/wiki/HTTP)/[XML](https://zh.wikipedia.org/wiki/XML)和[JSON](https://zh.wikipedia.org/wiki/JSON)的[API](https://zh.wikipedia.org/wiki/API)。 Solr强大的外部配置功能使得无需进行Java编码，便可对其进行调整以适应多种类型的应用程序。Solr有一个插件架构，以支持更多的高级定制。

- Near Realtime (NRT)

  Elasticsearch是一个近乎实时的搜索平台。这意味着从索引文档到可以搜索的时间只有轻微的延迟（通常是1秒

- Cluster

  集群是一个或多个节点(服务器)的集合，它们共同保存你的整个数据，并提供跨所有节点的联合索引和搜索功能。一个集群由一个唯一的名称标识，默认这个唯一标识的名称是"elasticsearch"。这个名称很重要，因为如果节点被设置为按其名称加入集群，那么节点只能是集群的一部分。

  确保不要在不同的环境中用相同的集群名称，否则可能导致节点加入到错误的集群中。例如，你可以使用"logging-dev", "logging-test", "logging-prod"分别用于开发、测试和正式集群的名字。

- Node

  节点是一个单独的服务器，它是集群的一部分，存储数据，并参与集群的索引和搜索功能。就像集群一样，节点由一个名称来标识，默认情况下，该名称是在启动时分配给节点的随机通用唯一标识符(UUID)。如果不想用默认的节点名，可以定义任何想要的节点名。这个名称对于管理来说很重要，因为你希望识别网络中的哪些服务器对应于你的Elasticsearch集群中的哪些节点。

  一个节点可以通过配置集群名称来加入到一个特定的集群中。默认情况下，每个节点都被设置加入到一个名字叫"elasticsearch"的集群中，这就意味着如果你启动了很多个节点，并且假设它们彼此可以互相发现，那么它们将自动形成并加入到一个名为"elasticsearch"的集群中。

  一个集群可以有任意数量的节点。此外，如果在你的网络上当前没有运行任何节点，那么此时启动一个节点将默认形成一个单节点的名字叫"elasticsearch"的集群。

- Index

  索引是具有某种相似特征的文档的集合。例如，你可以有一个顾客数据索引，产品目录索引和订单数据索引。索引有一个名称标识，该名称用于在对其中的文档执行索引、搜索、更新和删除操作时引用索引。

- Document

  文档是可以被索引的基本信息单元，类似数据库的单条表数据

### Install

- Docker

  ```bash
  ## Elasticsearch Dockerfile
  
  FROM docker.elastic.co/elasticsearch/elasticsearch:6.6.0
  
  EXPOSE 9200 9300
  ```

  ```bash
  ## Kibana Dockerfile
  
  FROM docker.elastic.co/kibana/kibana:6.6.0
  
  EXPOSE 5601
  ```

- [Others](https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started-install.html)

### Build & Start

```bash
docker-compose up -d elasticsearch kibana
```

### Check

- Elasticsearch

  Open `localhost:9200` with your browser

  ```bash
  {
    "name": "aJRPWzC",
    "cluster_name": "laradock-cluster",
    "cluster_uuid": "Bd8hjDXlTNSS5yQIondlDg",
    "version": {
      "number": "6.6.0",
      "build_flavor": "default",
      "build_type": "tar",
      "build_hash": "a9861f4",
      "build_date": "2019-01-24T11:27:09.439740Z",
      "build_snapshot": false,
      "lucene_version": "7.6.0",
      "minimum_wire_compatibility_version": "5.6.0",
      "minimum_index_compatibility_version": "5.0.0"
    },
    "tagline": "You Know, for Search"
  }
  ```

- Kibana

  Open `localhost:5601` with your browser

### API

#### Index

- Create Index

  ```bash
  PUT index_name
  
  ## PUT twitter
  {
    "acknowledged" : true,
    "shards_acknowledged" : true,
    "index" : "twitter"
  }
  ```

- Index List

  ```bash
  curl -X GET "localhost:9200/_cat/indices?v"
  ```

#### Document

- Create Document

  `PUT index_name/_doc`

  指定id

  ```bash
  ## 指定id
  PUT twitter/_doc/1
  {
      "user" : "kimchy",
      "post_date" : "2009-11-15T14:12:12",
      "message" : "trying out Elasticsearch"
  }
  ```

  Result:

  ```bash
  {
    "_index" : "twitter",
    "_type" : "_doc",
    "_id" : "1",
    "_version" : 1,
    "result" : "created",
    "_shards" : {
      "total" : 2,
      "successful" : 1,
      "failed" : 0
    },
    "_seq_no" : 0,
    "_primary_term" : 1
  }
  ```

  不指定id

  `POST index_name/_doc`

  ```bash
  POST twitter/_doc/
  {
      "user" : "kimchy",
      "post_date" : "2009-11-15T14:12:12",
      "message" : "trying out Elasticsearch"
  }
  ```

  ```bash
  {
    "_index" : "twitter",
    "_type" : "_doc",
    "_id" : "TGap7GgBR1R5Vn49jL0L",
    "_version" : 1,
    "result" : "created",
    "_shards" : {
      "total" : 2,
      "successful" : 1,
      "failed" : 0
    },
    "_seq_no" : 0,
    "_primary_term" : 1
  }
  ```

- Update Document

  `PUT index_name/_doc/primark_key`

  ```bash
  PUT twitter/_doc/1
  {
      "user":"new-kimchy"
  }
  ```

  Result:

  ```bash
  {
      "_index": "twitter",
      "_type": "_doc",
      "_id": "1",
      "_version": 2,
      "result": "updated",
      "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
      },
      "_seq_no": 1,
      "_primary_term": 1
  }
  ```

- Delete Document

  `DELETE index_name/_doc/primark_key`

  ```bash
  DELETE /twitter/_doc/2
  ```

  Result:

  ```bash
  {
      "_index": "twitter",
      "_type": "_doc",
      "_id": "2",
      "_version": 3,
      "result": "deleted",
      "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
      },
      "_seq_no": 4,
      "_primary_term": 1
  }
  ```

- Search Document

  - Easy Search

    `GET /index_name/_search?q=key:value`

    ```bash
    GET /twitter/_search?q=user:kimchy
    ```

    Result

    ```bash
    {
        "took": 35,
        "timed_out": false,
        "_shards": {
            "total": 5,
            "successful": 5,
            "skipped": 0,
            "failed": 0
        },
        "hits": {
            "total": 3,
            "max_score": 0.2876821,
            "hits": [
                {
                    "_index": "twitter",
                    "_type": "_doc",
                    "_id": "TGap7GgBR1R5Vn49jL0L",
                    "_score": 0.2876821,
                    "_source": {
                        "user": "kimchy",
                        "post_date": "2009-11-15T14:12:12",
                        "message": "trying out Elasticsearch"
                    }
                },
                {
                    "_index": "twitter",
                    "_type": "_doc",
                    "_id": "1",
                    "_score": 0.2876821,
                    "_source": {
                        "user": "new-kimchy"
                    }
                },
                {
                    "_index": "twitter",
                    "_type": "_doc",
                    "_id": "3",
                    "_score": 0.2876821,
                    "_source": {
                        "user": "kimchy",
                        "post_date": "2009-11-15T14:12:12",
                        "messages": "trying out Elasticsearch"
                    }
                }
            ]
        }
    }
    ```

  - Request Body Search

    ES 提供了一种JSON风格的 Query DSL (domain specific language)

    使用request body 传递参数，更加灵活

    ```bash
    GET /twitter/_search
    {
        "query" : {
            "term" : { "user" : "kimchy" }
        }
    }
    ```

    Result

    ```bash
    {
      "took" : 3,
      "timed_out" : false,
      "_shards" : {
        "total" : 5,
        "successful" : 5,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : 3,
        "max_score" : 0.2876821,
        "hits" : [
          {
            "_index" : "twitter",
            "_type" : "_doc",
            "_id" : "TGap7GgBR1R5Vn49jL0L",
            "_score" : 0.2876821,
            "_source" : {
              "user" : "kimchy",
              "post_date" : "2009-11-15T14:12:12",
              "message" : "trying out Elasticsearch"
            }
          },
          {
            "_index" : "twitter",
            "_type" : "_doc",
            "_id" : "1",
            "_score" : 0.2876821,
            "_source" : {
              "user" : "new-kimchy"
            }
          },
          {
            "_index" : "twitter",
            "_type" : "_doc",
            "_id" : "3",
            "_score" : 0.2876821,
            "_source" : {
              "user" : "kimchy",
              "post_date" : "2009-11-15T14:12:12",
              "messages" : "trying out Elasticsearch"
            }
          }
        ]
      }
    }
    ```



### Test

数据量为 223479条

```bash
## mysql 用时:11.081s
SELECT * FROM company WHERE company_name LIKE "%建%"

## es < 1s
localhost:9200/company-100/_doc/_search?q=company_name:建
```



### Extra

- 本次测试223479条mysql数据为60.58MB，导入ES后,查看`http://localhost:9200/_cat/indices?v`，数据为106.6MB

### References

- [http://solr-vs-elasticsearch.com/](http://solr-vs-elasticsearch.com/)
- [elasticsearch document](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [为什么Elasticsearch/Lucene检索可以比MySQL快](https://blog.csdn.net/qq924862077/article/details/80382634)
- [百度指数对比](https://index.baidu.com/v2/main/index.html#/trend/elasticsearch?words=elasticsearch,solr)





