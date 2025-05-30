# Database 



---

![· MySQL ／ PosgreSQL 等 SQL 数 据 库 的 性 能 · 约 1k QPS 这 个 级 别 · MongoDB / Cassandra 等 硬 盘 型 NoSQL 数 据 库 的 性 能 · 约 10k QPS 这 个 级 别 （ · Memcached / Redis 等 内 存 型 NoSQL 数 据 库 的 性 能 · 100k 、 1m QPS 这 个 级 别 · 以 上 数 据 根 据 机 器 性 能 和 硬 盘 数 量 及 硬 盘 读 写 速 度 会 有 区 别 ](../media/Basic-Database-image1.png)



![数 据 库 选 择 原 则 2 需 要 支 持 Transaction 的 话 不 能 选 NoSQL ](../media/Basic-Database-image2.png)



![数 据 库 选 择 原 则 3 你 想 不 想 偷 懒 很 大 程 度 决 定 了 选 什 么 数 据 库 SQL 更 成 熟 帮 你 做 了 很 多 事 儿 NoSQL 很 多 事 儿 都 要 亲 力 亲 为 (Serialization, MultiIndexes, Sharding) ](../media/Basic-Database-image3.png)



![数 据 库 选 择 原 则 4 如 果 想 省 点 服 务 器 获 得 更 高 的 性 能 NoSQL 就 更 好 硬 盘 型 的 NoSQL 比 SQL 一 般 都 要 快 10 倍 以 上 ](../media/Basic-Database-image4.png)



![](../media/Basic-Database-image5.png)







![SQL vs NoSQL --- ? • Transaction ? • • SQL Query? • NoSQLfiSQL • Web Framework 5 SQL • SQL auto-increment fi Sequential ID • NoSQLfiID#-FZ Sequential fi ](../media/Basic-Database-image6.png)



![QPS*Q Web Server / Database • ---S Web Server 1k fi QPS • ---S SQL Database 1k fi JOIN INDEX queryEtü$fiiä, • ---S NoSQL Database (Cassandra) 10k fi QPS • ---S NoSQL Database (Memcached) 1M fi QPS ](../media/Basic-Database-image7.png)



![Web server 1k QPS SQL DB 1k QPS NoSQL DB( Cassandra, MongoDB NoSQL DB (Redis) 100k QPS NoSQL DB (memcached) 1M QPS ..) 10k ops ](../media/Basic-Database-image8.png)



![](../media/Basic-Database-image9.png)











from message :



For a write heavy system with a lot of data, traditional database usually don`t perform well. Every write is not just an append to a table but also an update to multiple index which might require locking and so it might affect with reads and other writes.







Compare SQL and no SQL



For example, if we want to build a database for resume application

For SQL databse: we need server table,

User table

User id, first name, last name, company id , school id

Company table

School table

But if you store the information to the no sql table such as the document table:

Key will be the user id and value will be whole information about the user like xml or json format

Should: ..

Company..

The benefit of document database is easily to read, don`t need join multiple table like sql database

Disadvantage is not easy to change the data and we need load the entire document even you just need small part

















