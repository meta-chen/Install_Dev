---

---

# ElasticSearch 

#### 主要集成解决方案

ELK（ElasticSearch、LogStash数据收集和日志解析引擎、Kibana分析和可视化平台）



#### ElasticSearch 与 MySQL 对比 :

| MySQL              | ElasticSearch  |
| ------------------ | -------------- |
| database（数据库） | index(索引库)  |
| table(表)          | type(类型)     |
| row(行)            | document(文档) |
| column（列）       | field(字段)    |



#### RESTFul风格

##### REST风格

是一种软件架构风格，主要用于客户端与服务器之间的交互

比如URL路径（资源名称）设计原则：

- **好：** / users / 12345
- **差：** / api？type = user＆id = 23

![img](pictures\8871747-aec15633c4f81746.webp)

##### ES常用内置REST接口

| URL接口          | 说明                                                |
| ---------------- | --------------------------------------------------- |
| /_aliases        | 获取或者操作索引的别名                              |
| /index/_search   | 搜索指定索引下的数据                                |
| /index/          | 查看指定索引详细信息                                |
| /index/type/     | 创建或者操作类型(表)                                |
| /index/_mapping  | 创建或者操作 mapping                                |
| /index/_settings | 创建或者操作 设置(number_of_shards是不可更改的)     |
| /index/_open     | 打开指定被关闭的索引                                |
| /index/_close    | 关闭指定索引                                        |
| /index/_refresh  | 刷新索引-使新加内容对搜索可见，不保证数据被写入磁盘 |
| /index/_flush    | 刷新索引-会触发lucene提交                           |



#### ES集群监控

ES 中使用三种颜色表示状态：使用`bigdesk`插件查看

| 颜色   | 状态                                   |
| ------ | -------------------------------------- |
| Green  | 所有主分片和副分片都可用               |
| Yellow | 所有主分片可用，但不是所有副分片都可用 |
| Red    | 不是所有主分片可用                     |



#### ES配置文件详细内容

```yaml
cluster.name: elasticsearch
配置es的集群名称，默认是elasticsearch，es会自动发现在同一网段下的es，如果在同一网段下有多个集群，就可以用这个属性来区分不同的集群。

node.name: "Franz Kafka"
节点名，默认随机指定一个name列表中名字，该列表在es的jar包中config文件夹里name.txt文件中，其中有很多作者添加的有趣名字。

node.master: true
指定该节点是否有资格被选举成为node，默认是true，es是默认集群中的第一台机器为master，如果这台机挂了就会重新选举master。

node.data: true
指定该节点是否存储索引数据，默认为true。

index.number_of_shards: 5
设置默认索引分片个数，默认为5片。

index.number_of_replicas: 1
设置默认索引副本个数，默认为1个副本。

path.conf: /path/to/conf
设置配置文件的存储路径，默认是es根目录下的config文件夹。

path.data: /path/to/data
设置索引数据的存储路径，默认是es根目录下的data文件夹，可以设置多个存储路径，用逗号隔开，例：
path.data: /path/to/data1,/path/to/data2

path.work: /path/to/work
设置临时文件的存储路径，默认是es根目录下的work文件夹。

path.logs: /path/to/logs
设置日志文件的存储路径，默认是es根目录下的logs文件夹

path.plugins: /path/to/plugins
设置插件的存放路径，默认是es根目录下的plugins文件夹

bootstrap.mlockall: true
设置为true来锁住内存。因为当jvm开始swapping时es的效率 会降低，所以要保证它不swap，可以把ES_MIN_MEM和ES_MAX_MEM两个环境变量设置成同一个值，并且保证机器有足够的内存分配给es。 同时也要允许elasticsearch的进程可以锁住内存，linux下可以通过`ulimit -l unlimited`命令。

network.bind_host: 192.168.0.1
设置绑定的ip地址，可以是ipv4或ipv6的，默认为0.0.0.0。

network.publish_host: 192.168.0.1
设置其它节点和该节点交互的ip地址，如果不设置它会自动判断，值必须是个真实的ip地址。

network.host: 192.168.0.1
这个参数是用来同时设置bind_host和publish_host上面两个参数。

transport.tcp.port: 9300
设置节点间交互的tcp端口，默认是9300。

transport.tcp.compress: true
设置是否压缩tcp传输时的数据，默认为false，不压缩。

http.port: 9200
设置对外服务的http端口，默认为9200。

http.max_content_length: 100mb
设置内容的最大容量，默认100mb

http.enabled: false
是否使用http协议对外提供服务，默认为true，开启。

gateway.type: local
gateway的类型，默认为local即为本地文件系统，可以设置为本地文件系统，分布式文件系统，hadoop的HDFS，和amazon的s3服务器，其它文件系统的设置方法下次再详细说。

gateway.recover_after_nodes: 1
设置集群中N个节点启动时进行数据恢复，默认为1。

gateway.recover_after_time: 5m
设置初始化数据恢复进程的超时时间，默认是5分钟。

gateway.expected_nodes: 2
设置这个集群中节点的数量，默认为2，一旦这N个节点启动，就会立即进行数据恢复。

cluster.routing.allocation.node_initial_primaries_recoveries: 4
初始化数据恢复时，并发恢复线程的个数，默认为4。

cluster.routing.allocation.node_concurrent_recoveries: 2
添加删除节点或负载均衡时并发恢复线程的个数，默认为4。

indices.recovery.max_size_per_sec: 0
设置数据恢复时限制的带宽，如入100mb，默认为0，即无限制。

indices.recovery.concurrent_streams: 5
设置这个参数来限制从其它分片恢复数据时最大同时打开并发流的个数，默认为5。

discovery.zen.minimum_master_nodes: 1
设置这个参数来保证集群中的节点可以知道其它N个有master资格的节点。默认为1，对于大的集群来说，可以设置大一点的值（2-4）

discovery.zen.ping.timeout: 3s
设置集群中自动发现其它节点时ping连接超时时间，默认为3秒，对于比较差的网络环境可以高点的值来防止自动发现时出错。

discovery.zen.ping.multicast.enabled: false
设置是否打开多播发现节点，默认是true。

discovery.zen.ping.unicast.hosts: ["host1", "host2:port", "host3[portX-portY]"]
设置集群中master节点的初始列表，可以通过这些节点来自动发现新加入集群的节点。

下面是一些查询时的慢日志参数设置
index.search.slowlog.level: TRACE
index.search.slowlog.threshold.query.warn: 10s
index.search.slowlog.threshold.query.info: 5s
index.search.slowlog.threshold.query.debug: 2s
index.search.slowlog.threshold.query.trace: 500ms

index.search.slowlog.threshold.fetch.warn: 1s
index.search.slowlog.threshold.fetch.info: 800ms
index.search.slowlog.threshold.fetch.debug:500ms
index.search.slowlog.threshold.fetch.trace: 200ms
```



#### CURL

##### 简介

使用**命令行**的方式向远程的服务通过http协议发送GET/POST请求，并获得来自远程服务器的反馈。

##### 注意

在es7.0以上版本中，使用CURL命令，由于取消了type类型，所以查询的时候，type对应字段由`_doc`替代。

##### 常用的参数

| 参数 | 说明                                                       |
| ---- | ---------------------------------------------------------- |
| -X   | 用来后接请求参数，如：GET POST PUT DELETE等                |
| -d   | 用来标识客户端向远程服务器发送的数据，数据的格式一般是json |
| -H   | 用来标识请求头的信息                                       |

##### 创建索引库

```curl -XPUT "http://localhost:9200/index_name"``` note: windows中只能使用双引号

##### 添加及更新数据

```json
curl -H "Content-Type:application/json" -XPOST "http://localhost:9200/index_name/type_name/doc_name" -d
"
{
    "name" :"hadoop",
    "author":"Doug cutting"
}
"
///以上为linux下的命令，windows如下（json内容使用三个双引号!!!）：
curl -H "Content-Type:application/json" -XPOST "http://localhost:9200/bigdata/product/1" -d "{"""name""":"""hadoop""","""author""":"""Doug_Cutting"""}"
```

Note: 什么时候使用PUT什么时候使用POST

PUT是幂等方法，POST不是。

幂等的意思是指不管进行多少次操作，结果都一样，比如使用PUT修改一篇文章后，然后重复同样的操作，每次操作的结果并无不同（没有多余的修改），DELETE也一样。POST操作不是幂等，比如POST的重复加载问题，多次发出POST请求后，结果师创建了多个资源。

所以**PUT用于更新，POST用于创建**比较合适。

此外，创建使用PUT与POST都可，区别在于POST作用域为一个集合上（/articles），而PUT为一个具体资源上（/articles/123），很多资源使用数据库自增主键作为标识，此时用PUT。而资源的标识只能由服务端提供时，只能用POST。

##### ES索引及索引库创建规则

1. 索引库名称必须全部小写，不能以下划线开头（下划线开头的基本为内部方法），也不能包含逗号
2. 如果没有明确指定索引数据的ID（文档document的id），那么会随机生成一个ID，需要使用POST参数

**局部更新**：只更新某个字段Field，使用POST与`_update`组合

```json
curl -H "Content-Type:application/json" -XPOST "http://localhost:9200/bigdata/product/1/_update" -d "{"""name""":"""hadoop""","""author""":"""周杰伦"""}""
```

Note： _update 表示更新的动作，在url中，以下划线开头的是动作

##### 删除数据

```bash
curl -XDELETE "http://localhost:9200/bigdata/product/1?pretty"
// pretty后缀表示以json标准格式返回数据到控制台
```

##### 批量处理操作

步骤：

- 新建一个索引库比如bank

- 将待处理的数据上传到**指定目录**下

- 进行批量操作

  ```bash
  curl -H "Content-Type:application/json" -XPOST  "localhost:9200/bank/account/_bulk" --data-binary @/path/to/data/accounts.json
  ```

Note:

- bulk请求可以在URL中声明 /index或者 /index/type

- bulk一次可处理数据量：bulk会将要处理的数据载入内存中，所以数据量是有限的，最佳的数据两不是一个确定的数据，它取决于你的硬件，你的文档大小以及复杂性，你的索引以及搜索的负载。

  一般建议是1000-5000个文档，大小建议是5-15MB，默认不能超过100M，可以在es的配置文件（即$ES_HOME下的config下的elasticsearch.yml）中，bulk的线程池配置是内核数+1。



#### ES插件

##### BigDesk Plugin

节点的实时状态监控，包括jvm的情况，Linux的情况，Elastic Search的情况

##### ES-Head Plugin

方便对ES进行各种操作的客户端工具

### ES核心概念

- ES 核心概念之Cluster（集群）