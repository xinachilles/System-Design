# CAP



---

distributed system, there are three properties are very important.

you can only choose two out of these three properties. You can't guarantee all the three. The three properties are: consistency, availability, and partition-tolerance.





Partition-tolerance says that when the system is partitioned, when the network is partitioned into say two parts that cannot talk with each other, but client able to talk to any of them of both them

the system continues to work guarantee both the consistency and the availability



partition is important because partitions can happen across data centers when the internet gets disconnected. system to continue functioning and providing consistency and availability under these partition scenarios.



partition-tolerance is really, really important in today







Consistency essentially says that even though there are multiple clients that are reading and writing the data, all the clients, see the same data at any given point of time. and that the reads or any client return the latest written value by that particular client.



Availability says that the system allows read and write operations on all the keys all the time, and these operations return very, very quickly.







So the CAP theorem essentially says that you cannot guarantee all the three, you have to choose, at most, two out of the three.





Cassandra always chooses availability. So it chooses availability A, and partition-tolerance P. Which means that it has to punt on the consistency. It can only provide weaker forms of consistency, and it provides a weak form known as eventual



Cassandra does not provide strong notions of consistency. Traditional relation databases, uh, prefer strong consistency instead over availability or insert availability whenever you have a partition.



So relation databases prefer consistency and availability in in the-in scenarios, where they are single server and they really don't care about partitions.



Cassandra prefer availability and partition-tolerance, and they provide weaker notions of consistency





High Concurrency implements by scale out

we also can use multiple nginx and using Round Robin DNS to distribute different request to different nginx



![Scale 一 一 如 何 扩 展 ？ · 什 么 时 候 需 要 多 台 数 据 库 服 务 器 ？ · Cache 资 源 不 够 · 写 操 作 越 来 越 多 ． 越 来 越 多 的 请 求 无 法 通 过 Cache 满 足 · 增 加 多 台 数 据 库 服 务 器 可 以 优 化 什 么 ？ ， 解 决 " 存 不 下 " 的 问 题 一 一 Storage 的 角 度 · 解 决 " 忙 不 过 " 的 问 题 ---QPS 的 角 度 · Tiny URL 主 要 是 什 么 问 题 ？ 如 何 将 数 据 分 配 到 多 台 机 器 ？ ](../media/Basic-CAP-image1.png)







High available

for nginx layer,we can use two service one is id generate service and another is id generate shadow



for web service layer, they will send the heart beat to ngix



for cache, we don`t ask the cache has high available, we just got a cache miss will cache is not available.



for database, we use master and multiple slaver model, at least two slaver database



for master database, we can use master and shadow or promote a slaver database to master





