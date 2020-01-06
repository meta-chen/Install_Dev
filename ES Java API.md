## ES Java API

官方提供了两种java语言的API，一种是Java Transport Client，一种是Java REST Client。

![Java API](E:\PycharmProjects\SomeTips\pictures\Java API.png)

Java High Level REST Client是在Java Low Level REST Client的基础上做了封装，使其以更加面向对象和操作更加便利的方式调用elasticsearch服务。


官方推荐使用Java High Level REST Client，因为在实际使用中，Java Transport Client在大并发的情况下会出现连接不稳定的情况。

> We plan on **deprecating the `TransportClient` in Elasticsearch 7.0** and removing it completely in 8.0. Instead, you should be using the [Java High Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.8/java-rest-high.html), which executes HTTP requests rather than serialized Java requests. The [migration guide](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.8/java-rest-high-level-migration.html) describes all the steps needed to migrate.
>
> The Java High Level REST Client currently has support for the more commonly used APIs, but there are a lot more that still need to be added. You can help us prioritise by telling us which missing APIs you need for your application by adding a comment to this issue: [Java high-level REST client completeness](https://github.com/elastic/elasticsearch/issues/27205).
>
> Any missing APIs can always be implemented today by using the [low level Java REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-low.html) with JSON request and response bodies.

