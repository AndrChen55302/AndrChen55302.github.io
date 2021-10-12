---
title: Solr8.x使用教程
date: 2021-10-12
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1634032448000.webp
tags: solr使用教程
categories: solr
---

# Solr8.x使用教程

## 搜索引擎简介

### 1、Elasticsearch简介

Elasticsearch是一个实时分布式搜索和分析引擎。它让你以前所未有的速度处理大数据成为可能。

它用于全文搜索、结构化搜索、分析以及将这三者混合使用：

维基百科使用Elasticsearch提供全文搜索并高亮关键字，以及输入实时搜索(search-asyou-type)和搜索纠错(did-you-mean)等搜索建议功能。

英国卫报使用Elasticsearch结合用户日志和社交网络数据提供给他们的编辑以实时的反馈，以便及时了解公众对新发表的文章的回应。
StackOverflow结合全文搜索与地理位置查询，以及more-like-this功能来找到相关的问题和答案。

Github使用Elasticsearch检索1300亿行的代码。

但是Elasticsearch不仅用于大型企业，它还让像DataDog以及Klout这样的创业公司将最初的想法变成可扩展的解决方案。Elasticsearch可以在你的笔记本上运行，也可以在数以百计的服务器上处理PB级别的数据 。

Elasticsearch是一个基于Apache Lucene(TM)的开源搜索引擎。无论在开源还是专有领域，Lucene可以被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。

但是，Lucene只是一个库。想要使用它，你必须使用Java来作为开发语言并将其直接集成到你的应用中，更糟糕的是，Lucene非常复杂，你需要深入了解检索的相关知识来理解它是如何工作的。

Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的 RESTful API 来隐藏Lucene的复杂性，从而让全文搜索变得简单。

### 2、Solr简介

Solr 是Apache下的一个顶级开源项目，采用Java开发，它是基于Lucene的全文搜索服务器。Solr提供了比Lucene更为丰富的查询语言，同时实现了可配置、可扩展，并对索引、搜索性能进行了优化

Solr可以独立运行，运行在Jetty、Tomcat等这些Servlet容器中，Solr 索引的实现方法很简单，用 POST 方法向 Solr 服务器发送一个描述 Field 及其内容的 XML 文档，Solr根据xml文档添加、删除、更新索引 。Solr 搜索只需要发送 HTTP GET 请求，然后对 Solr 返回Xml、json等格式的查询结果进行解析，组织页面布局。Solr不提供构建UI的功能，Solr提供了一个管理界面，通过管理界面可以查询Solr的配置和运行情况。

solr是基于lucene开发企业级搜索服务器，实际上就是封装了lucene。

Solr是一个独立的企业级搜索应用服务器，它对外提供类似于Web-service的API接口。用户可以通过http请求，向搜索引擎服务器提交一定格式的文件，生成索引；也可以通过提出查找请求，并得到返回结果。

### 3、Lucene简介

Lucene是apache软件基金会4 jakarta项目组的一个子项目，是一个开放源代码的全文检索引擎工具包，但它不是一个完整的全文检索引擎，而是一个全文检索引擎的架构，提供了完整的查询引擎和索引引擎，部分文本分析引擎（英文与德文两种西方语言）。Lucene的目的是为软件开发人员提供一个简单易用的工具包，以方便的在目标系统中实现全文检索的功能，或者是以此为基础建立起完整的全文检索引擎。Lucene是一套用于全文检索和搜寻的开源程式库，由Apache软件基金会支持和提供。Lucene提供了一个简单却强大的应用程式接口，能够做全文索引和搜寻。在Java开发环境里Lucene是一个成熟的免费开源工具。就其本身而言，Lucene是当前以及最近几年最受欢迎的免费Java信息检索程序库。人们经常提到信息检索程序库，虽然与搜索引擎有关，但不应该将信息检索程序库与搜索引擎相混淆。

Lucene是一个全文检索引擎的架构。那什么是全文搜索引擎？

全文搜索引擎是名副其实的搜索引擎，国外具代表性的有Google、Fast/AllTheWeb、AltaVista、Inktomi、Teoma、WiseNut等，国内著名的有百度（Baidu）。它们都是通过从互联网上提取的各个网站的信息（以网页文字为主）而建立的数据库中，检索与用户查询条件匹配的相关记录，然后按一定的排列顺序将结果返回给用户，因此他们是真正的搜索引擎。

从搜索结果来源的角度，全文搜索引擎又可细分为两种，一种是拥有自己的检索程序（Indexer），俗称“蜘蛛”（Spider）程序或“机器人”（Robot）程序，并自建网页数据库，搜索结果直接从自身的数据库中调用，如上面提到的7家引擎；另一种则是租用其他引擎的数据库，并按自定的格式排列搜索结果，如Lycos引擎。

### 4、Elasticsearch和Solr比较

![img](https://gitee.com/lexizhi/blogimg/raw/master/2021/354604-20180122010705803-1082290454.png)

![img](https://gitee.com/lexizhi/blogimg/raw/master/2021/354604-20180122010730865-1548826450.png)

![img](https://gitee.com/lexizhi/blogimg/raw/master/2021/354604-20180122010754100-1951694800.png)

![img](https://gitee.com/lexizhi/blogimg/raw/master/2021/354604-20180122011131225-347761833.png)

### 5、ElasticSearch vs Solr 总结

　　（1）es基本是开箱即用，非常简单；Solr安装略微复杂一丢丢。

　　（2）Solr 利用 Zookeeper 进行分布式管理，而 Elasticsearch 自身带有分布式协调管理功能。

　　（3）Solr 支持更多格式的数据，比如JSON、XML、CSV，而 Elasticsearch 仅支持json文件格式。

　　（4）Solr 官方提供的功能更多，而 Elasticsearch 本身更注重于核心功能，高级功能多有第三方插件提供，例如图形化界面需要kibana友好支撑

　　（5）Solr 查询快，但更新索引时慢（即插入删除慢），用于电商等查询多的应用；

　　　　 ES建立索引快（即查询慢），即实时性查询快，用于facebook新浪等搜索。

　　   Solr 是传统搜索应用的有力解决方案，但 Elasticsearch 更适用于新兴的实时搜索应用。

　　（6）Solr比较成熟，有一个更大，更成熟的用户、开发和贡献者社区，而 Elasticsearch相对开发维护者较少，更新太快，学习使用成本较高。

### 6、Hermes

在整理solr和es资料的时候意外发现了一篇文章[ Hermes与开源的Solr、ElasticSearch的不同](https://www.csdn.net/article/2014-12-22/2823243)，中提到了hermes和他们两者的对比，于是摘抄了下面的部分文字，由于首次见到没有什么了解，今摘抄下了给广大读者有兴趣的朋友吧1

Solr\ES ：偏重于为小规模的数据提供全文检索服务；Hermes：则更倾向于为大规模的数据仓库提供索引支持，为大规模数据仓库提供即席分析的解决方案，并降低数据仓库的成本，Hermes数据量更“大”。

#### Solr、ES的使用特点如下：

1. 源自搜索引擎，侧重搜索与全文检索。
2. 数据规模从几百万到千万不等，数据量过亿的集群特别少。

#### Hermes:的使用特点如下：

1. 一个基于大索引技术的海量数据实时检索分析平台。侧重数据分析。 
2. 数据规模从几亿到万亿不等。最小的表也是千万级别。在腾讯17 台TS5机器，就可以处理每天450亿的数据(每条数据1kb左右)，数据可以保存一个月之久。

## Solr8.x实战

### 1、下载安装solr8

下载页面：https://solr.apache.org/downloads.html

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012124342982.png" alt="image-20211012124342982" style="zoom:80%;" />


【linux版】：https://dlcdn.apache.org/lucene/solr/8.10.0/solr-8.10.0.tgz

【Windows版】：https://dlcdn.apache.org/lucene/solr/8.10.0/solr-8.10.0.zip

### 2、安装solr8

> **注意：**需要JAVA_Home的配置

将下载好的安装包解压到一个**没有空格，没有中文**的路径

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012124143839.png" alt="image-20211012124143839" style="zoom: 80%;" />

### 3、solr常用命令

#### （1）启动solr

```cmd
solr.cmd start
```

在浏览器访问：`localhost:8093`，打开如下界面，solr就启动成功了。

![image-20211012124915529](https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012124915529.png)

#### （2）重启solr

```cmd
solr.cmd restart -p 指定端口
```

#### （3）关闭solr

```cmd
solr.cmd stop --all
```

### 4、新建core

> 相当于SQL的视图

#### （1）命令创建core

```cmd
.\solr.cmd create -c demo_core
```

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012133425411.png" alt="image-20211012133425411" style="zoom:80%;" />

#### （2）手工创建core

在solr目录中的\server\solr文件夹中创建一个名为`test-core`的文件夹。然后把\server\solr\configsets\sample_techproducts_configs下面的conf文件夹复制到刚才创建的`test-core`文件夹下面。

![image-20211012125452544](https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012125452544.png)

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012125527272.png" alt="image-20211012125527272" style="zoom: 80%;" />

　　完成上面操作之后启动solr，启动成功之后打开浏览器进入http://127.0.0.1:8983/solr/#/地址，就可以看见solr的管理界面。点击左侧的Core Admin，在新页面的name和instanceDir输入框中分别修改为刚才创建的文件夹名称`test-core`，然后点击下面Add Core按钮即可成功创建core。

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012125723876.png" alt="image-20211012125723876" style="zoom:80%;" />

　　页面显示创建成功之后，会在你刚才创建的`test-core`文件夹下面生成一个data文件夹和一个core.properties文件，这样即表示创建成功。

![image-20211012130340424](https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012130340424.png)

### 5、连接mysql数据库

#### （1）拷贝相关jar包

- solr-dataimporthandler-8.10.0.jar
- solr-dataimporthandler-extras-8.10.0.jar
- mysql-connector-java-xxx.jar

将`D:\solr-8.10.0`目录下的`dist`目录中的两个jar：`solr-dataimporthandler-8.10.0.jar `和 `solr-dataimporthandler-extras-8.10.0.jar`以及 `mysql-connector-java-xxx.jar` 拷贝到`D:\solr-8.10.0\server\solr-webapp\webapp\WEB-INF\lib`文件夹下

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012131013573.png" alt="image-20211012131013573" style="zoom:80%;" />

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012130946111.png" alt="image-20211012130946111" style="zoom:80%;" />

#### （2）修改配置文件solrconfig.xml

修改`D:\solr-8.10.0\server\solr\hotel_core\conf\solrconfig.xml`

打开文件，找到下面语句的位置

```xml
<requestHandler name="/select" class="solr.SearchHandler">
```

在该语句的上方添加如下语句：

```xml
<requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler">
　　     <lst name="defaults">
　　        <str name="config">data-config.xml</str>
　　     </lst>
</requestHandler>
```

如下图所示：

![image-20211012131206470](https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012131206470.png)

#### （3）新建data-config.xml文件

在路径D:\solr-8.10.0\server\solr\hotel_core\conf下新建data-config.xml文件，文件内容如下：

> 根据现有数据库的情况，选择一个表进行练习

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<dataConfig>
	<dataSource type="JdbcDataSource" driver="com.mysql.cj.jdbc.Driver" url="jdbc:mysql://localhost:3306/itripdb" user="root" password="1234" encoding="UTF-8"/>
		<document>
			<entity name="itrip_user" processor="SqlEntityProcessor" pk="id" query="select * from itrip_user">
				<field name="id" column="id"/>
				<field name="userCode" column="userCode"/>
				<field name="userName" column="userName"/>
			</entity>
		</document>
</dataConfig>
```

#### （4）修改文件managed-schema文件

增加以下内容：

```xml
<field name="userCode" type="string" indexed="true" stored="true" multiValued="true"/>
<field name="userName" type="string" indexed="true" stored="true" multiValued="true"/>
```

### 6、solr导入mysql数据

#### （1）重启solr服务

#### （2）重启后打开浏览器进入到solr页面

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012132206789.png" alt="image-20211012132206789" style="zoom:80%;" />

#### （3）完成

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012132329088.png" alt="image-20211012132329088" style="zoom:80%;" />

#### （4）查询

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012132454752.png" alt="image-20211012132454752" style="zoom: 67%;" />

### 7、尝试分词操作

![image-20211012133857255](https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012133857255.png)

### 8、安装中文分词库

#### （1）下载分词库

【下载地址】：https://search.maven.org/search?q=com.github.magese

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012134329749.png" alt="image-20211012134329749" style="zoom: 80%;" />

#### （2）配置分词库

将下载好的分词库拷贝到`D:/solr-8.10.0/server/solr-webapp/webapp/WEB-INF/lib` 路径下

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012134801918.png" alt="image-20211012134801918" style="zoom:80%;" />

打开`D:/solr-8.10.0/server/solr/hotel_core/managed-schema` 文件，在文件末尾`</schema>`之前添加以下代码：

```xml
<fieldType name="text_ik" class="solr.TextField">
    <analyzer type="index">
        <tokenizer class="org.wltea.analyzer.lucene.IKTokenizerFactory" useSmart="false" conf="ik.conf"/>
        <filter class="solr.LowerCaseFilterFactory"/>
    </analyzer>
    <analyzer type="query">
      <tokenizer class="org.wltea.analyzer.lucene.IKTokenizerFactory" useSmart="true" conf="ik.conf"/>
      <filter class="solr.LowerCaseFilterFactory"/>
    </analyzer>
</fieldType>
```

如下图所示：

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012135041623.png" alt="image-20211012135041623" style="zoom:80%;" />

重启solr服务，命令为

```cmd
solr.cmd restart
```

#### （3）使用手动安装的中文分词库进行分词操作

打开浏览器，重新输入语句，选择刚刚下载拷贝到solr下的ik

<img src="https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012135433497.png" alt="image-20211012135433497" style="zoom:80%;" />

分词结果如下：

![image-20211012135451523](https://gitee.com/lexizhi/blogimg/raw/master/2021/image-20211012135451523.png)
