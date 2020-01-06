## ES核心概念

### Cluster·集群

​		集群是一个或多个节点（服务器）的集合，这些节点将<u>共同拥有整个数据</u>，并跨越节点提供联合索引和搜索功能。集群由唯一名称标识，默认名称为”elasticsearch"，节点<u>只能通过集群名称加入集群</u>。

### Node·节点

​		节点就是一台服务器，多个节点组成集群。节点具有一个名称标识，默认情况下，该名称是启动时分配给节点的随即通用标识符（UUID）

### Index·索引

​		索引是具有某种相似特性的文档集合。在单个集群中，可以定义任意多个索引。

### Type·类型

​		ES7.0 版本后被移除

> ##### Why are mapping types being removed?

> Initially, we spoke about an “index” being similar to a “database” in an SQL database, and a “type” being equivalent to a “table”.

> This was a bad analogy that led to incorrect assumptions. In an SQL database, tables are independent of each other. The columns in one table have no bearing on columns with the same name in another table. This is not the case for fields in a mapping type.

> In an Elasticsearch index, fields that have the same name in different mapping types are backed by the same Lucene field internally. In other words, using the example above, the user_name field in the user type is stored in exactly the same field as the user_name field in the tweet type, and both user_name fields must have the same mapping (definition) in both types.

> This can lead to frustration when, for example, you want deleted to be a date field in one type and a boolean field in another type in the same index.

> On top of that, storing different entities that have few or no fields in common in the same index leads to sparse data and interferes with Lucene’s ability to compress documents efficiently.

> For these reasons, we have decided to remove the concept of mapping types from Elasticsearch.

### Document·文档

​		文档是可以被索引的基本信息单元。文档以JSON数据格式表示，索引中可以存储任意多个文档。

### Shards&Replicas·分片与副本

​		索引可能会存储大量数据，这些数据可能超出单个节点的硬件限制，因此ES可以将索引<u>水平切分</u>为多段（称为分片）。每个分片本身就是一个完全功能性和独立的索引，<u>可以分布在集群中任何节点上</u>。ES分配分片和将其文档聚合回搜索请求的机制完全由ES管理，对用户透明。

​		ES允许将分片复制一份或多份，但是<u>副本不会被分配到主分片所在节点</u>。副本亦可被访问以提高查询效率。

Note： 一个索引中最大可拥有文档数=（integer_max_value-128）

### Recovery·恢复

一个索引的分片分配到另外一个节点的过程；一般在快照恢复、索引副本数变更、节点故障、节点重启时发生。由于master保存整个集群的状态信息，因此可以判断出哪些shard需要做再分配，以及分配到哪个结点，例如:
	如果某个shard主分片在，副分片所在结点挂了，那么选择另外一个可用结点，将副分片分配	(allocate)上去，然后进行主从分片的复制。
	如果某个shard的主分片所在结点挂了，副分片还在，那么将副分片升级为主分片，然后做主	从分片复制。
	如果某个shard的主副分片所在结点都挂了，则暂时无法恢复，等待持有相关数据的结点重新	加入集群后，从该结点上恢复主分片，再选择另外的结点复制副分片。

### Gateway

用来对数据进行长持久化，另外，整个集群重启之后可以通过gateway重新恢复数据。

### Discovery·发现

- 发现节点
  - 单播
  - 配置文件
- Master选举
- 组成集群，在Master信息发生变化时及时更新。
- 故障检测