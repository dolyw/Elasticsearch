# 000-GettingStarted

> A Elasticsearch-Docker Project

### 安装本地Elasticsearch

当然首先要安装`JDK1.8`的环境及以上版本都行，不能低于`1.8`，安装`Windows`本地版，去`Elasticsearch`官网下载即可，不过找了很久都没有找到旧版本，最后下了最新版7.2，安装很简单，将下载的`zip`文件解压后，直接运行`bin`下的`elasticsearch.bat`这个文件即可启动，关闭窗口就是关闭服务，然后访问本机的`127.0.0.1:9200`即可，网页返回如下`JSON`
```json
{
  "name" : "WANG926454",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "ht8iAPewRDidZk-qbZ2Eig",
  "version" : {
    "number" : "7.2.0",
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "508c38a",
    "build_date" : "2019-06-20T15:54:18.811730Z",
    "build_snapshot" : false,
    "lucene_version" : "8.0.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

### 安装本地Elasticsearch-Head

一般情况下，我们都会通过一个可视化的工具来查看`ES`的运行状态和数据。这个工具我们一般选择[Head](https://github.com/mobz/elasticsearch-head)，`Elasticsearch-Head`依赖于`Node.js`，需要安装`Node.js`，查看`Github`介绍，该工具能直接对`Elasticsearch`的数据进行增删改查，因此存在安全性的问题，建议生产环境下不要使用该插件，`Node.js`版本必须`requires node >= 6.0`，简单的菜鸟教程安装就行:[https://www.runoob.com/nodejs/nodejs-install-setup.html](https://www.runoob.com/nodejs/nodejs-install-setup.html)，我Node.js环境的很早就已经安装了，执行下面命令先安装`grunt`

```base
npm install -g grunt-cli
```

安装完成查看版本

```base
grunt -version
```

显示版本号OK

```base
grunt-cli v1.3.2
```

去[Github](https://github.com/mobz/elasticsearch-head)下载`Elasticsearch-Head`工具，解压到你的`Elasticsearch`根路径下`D:\Tools\elasticsearch-7.2.0\elasticsearch-head-master`，修改`Gruntfile.js`配置文件，如下添加`hostname: '*'`

```base
connect: {
	server: {
		options: {
			hostname: '*',
			port: 9100,
			base: '.',
			keepalive: true
		}
	}
}
```

然后在`D:\Tools\elasticsearch-7.2.0\elasticsearch-head-master`目录先安装下启动运行`Head`插件

```javascript
npm install
grunt server or npm run start
```

进`http://localhost:9100`发现连接不上，还需要配置下`Elasticsearch`，修改`Elasticsearch`安装目录下的`config/elasticsearch.yml`配置文件，在最下面添加下面两句配置，开启跨域

```yml
http.cors.enabled: true 
http.cors.allow-origin: "*"
# node.master: true
# node.data: true
```

重新打开`elasticsearch.bat`，启动完成进去`http://localhost:9100`连接，OK

### Elasticsearch概念

`Elasticsearch`有几个核心概念，从一开始理解这些概念会对整个学习过程有莫大的帮助

#### 接近实时(NRT)

`Elasticsearch`是一个接近实时的搜索平台。这意味着，从索引一个文档直到这个文档能够被搜索到有一个轻微的延迟(通常是1秒)

#### 集群(cluster)

一个集群就是由一个或多个节点组织在一起，它们共同持有你整个的数据，并一起提供索引和搜索功能。一个集群由一个唯一的名字标识，这个名字默认就是`Elasticsearch`。这个名字是重要的，因为一个节点只能通过指定某个集群的名字，来加入这个集群。在产品环境中显式地设定这个名字是一个好习惯，但是使用默认值来进行测试/开发也是不错的。

#### 节点(node)

一个节点是你集群中的一个服务器，作为集群的一部分，它存储你的数据，参与集群的索引和搜索功能。和集群类似，一个节点也是由一个名字来标识的，默认情况下，这个名字是一个随机的漫威漫画角色的名字，这个名字会在启动的时候赋予节点。这个名字对于管理工作来说挺重要的，因为在这个管理过程中，你会去确定网络中的哪些服务器对应于`Elasticsearch`集群中的哪些节点。

一个节点可以通过配置集群名称的方式来加入一个指定的集群。默认情况下，每个节点都会被安排加入到一个叫做`Elasticsearch`的集群中，这意味着，如果你在你的网络中启动了若干个节点，并假定它们能够相互发现彼此，它们将会自动地形成并加入到一个叫做`Elasticsearch`的集群中。

在一个集群里，只要你想，可以拥有任意多个节点。而且，如果当前你的网络中没有运行任何`Elasticsearch`节点，这时启动一个节点，会默认创建并加入一个叫做`Elasticsearch`的集群。

#### 索引(index)

一个索引就是一个拥有几分相似特征的文档的集合。比如说，你可以有一个客户数据的索引，另一个产品目录的索引，还有一个订单数据的索引。一个索引由一个名字来标识(必须全部是小写字母的)，并且当我们要对对应于这个索引中的文档进行索引、搜索、更新和删除的时候，都要使用到这个名字。索引类似于关系型数据库中`Database`的概念。在一个集群中，如果你想，可以定义任意多的索引。

#### 类型(type)

在一个索引中，你可以定义一种或多种类型。一个类型是你的索引的一个逻辑上的分类/分区，其语义完全由你来定。通常，会为具有一组共同字段的文档定义一个类型。比如说，我们假设你运营一个博客平台并且将你所有的数据存储到一个索引中。在这个索引中，你可以为用户数据定义一个类型，为博客数据定义另一个类型，当然，也可以为评论数据定义另一个类型。类型类似于关系型数据库中`Table`的概念。

#### 文档(document)

一个文档是一个可被索引的基础信息单元。比如，你可以拥有某一个客户的文档，某一个产品的一个文档，当然，也可以拥有某个订单的一个文档。文档以`JSON`(`Javascript Object Notation`)格式来表示，而`JSON`是一个到处存在的互联网数据交互格式。

在一个`index/type`里面，只要你想，你可以存储任意多的文档。注意，尽管一个文档，物理上存在于一个索引之中，文档必须被索引/赋予一个索引的`type`。文档类似于关系型数据库中`Record`的概念。实际上一个文档除了用户定义的数据外，还包括`_index`、`_type`和`_id`字段。

#### 分片和复制(shards & replicas) 

一个索引可以存储超出单个结点硬件限制的大量数据。比如，一个具有10亿文档的索引占据`1TB`的磁盘空间，而任一节点都没有这样大的磁盘空间；或者单个节点处理搜索请求，响应太慢。

为了解决这个问题，`Elasticsearch`提供了将索引划分成多份的能力，这些份就叫做分片。当你创建一个索引的时候，你可以指定你想要的分片的数量。每个分片本身也是一个功能完善并且独立的“索引”，这个“索引”可以被放置到集群中的任何节点上。 

分片之所以重要，主要有两方面的原因：

* 允许你水平分割/扩展你的内容容量
* 允许你在分片（潜在地，位于多个节点上）之上进行分布式的、并行的操作，进而提高性能/吞吐量

至于一个分片怎样分布，它的文档怎样聚合回搜索请求，是完全由`Elasticsearch`管理的，对于作为用户的你来说，这些都是透明的。

在一个网络/云的环境里，失败随时都可能发生，在某个分片/节点不知怎么的就处于离线状态，或者由于任何原因消失了。这种情况下，有一个故障转移机制是非常有用并且是强烈推荐的。为此目的，`Elasticsearch`允许你创建分片的一份或多份拷贝，这些拷贝叫做复制分片，或者直接叫复制。复制之所以重要，主要有两方面的原因：

* 在分片/节点失败的情况下，提供了高可用性。因为这个原因，注意到复制分片从不与原/主要（original/primary）分片置于同一节点上是非常重要的。
* 扩展你的搜索量/吞吐量，因为搜索可以在所有的复制上并行运行

总之，每个索引可以被分成多个分片。一个索引也可以被复制0次（意思是没有复制）或多次。一旦复制了，每个索引就有了主分片（作为复制源的原来的分片）和复制分片（主分片的拷贝）之别。分片和复制的数量可以在索引创建的时候指定。在索引创建之后，你可以在任何时候动态地改变复制数量，但是不能改变分片的数量。

默认情况下，`Elasticsearch`中的每个索引被分片5个主分片和1个复制，这意味着，如果你的集群中至少有两个节点，你的索引将会有5个主分片和另外5个复制分片（1个完全拷贝），这样的话每个索引总共就有10个分片。一个索引的多个分片可以存放在集群中的一台主机上，也可以存放在多台主机上，这取决于你的集群机器数量。主分片和复制分片的具体位置是由ES内在的策略所决定的

#### 形象比喻

* 索引：含有相同属性的文档集合
* 类型：索引可以定义一个或多个类型，文档必须属于一个类型
* 文档：可以被索引的基础数据单位
* 分片：每个索引都有多个分片，每个分片都是`Lucene`索引
* 备份：拷贝一份分片就完成分片的备份

百货大楼里有各式各样的商品，例如书籍、笔、水果等。书籍可以根据内容划分成不同种类，如科技类、教育类、悬疑推理等。悬疑推理类的小说中比较有名气的有《福尔摩斯探案集》、《白夜行》等

* 百货大楼 –> `Elasticsearch`数据库
* 书籍 –> 索引
* 悬疑推理 –> 类型
* 白夜行 –> 文档

#### 数据结构表格对比

* `Mysql`和`Elasticsearch`对应关系

|Mysql|Elasticsearch|
|:- |:-: |
|Database |Index |
|Table |Type |
|Row |Document |
|Column |Field |
|Schema |Mapping |
|Index |Everything is indexed |
|SQL |Query DSL |
|SELECT * FROM table |GET http://... |
|UPDATE table SET |PUT http://... |

* `Mysql`和`MongoDB`对应关系

|Mysql|MongoDB|
|:- |:-: |
|Database |Database |
|Table |Collection |
|Row |Document |
|Column |Field |
