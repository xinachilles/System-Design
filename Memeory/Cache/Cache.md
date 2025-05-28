# Cache

Created: 2017-05-06 00:18:11 -0600

Modified: 2021-02-09 18:40:28 -0600

---

![Memcached DB 1 class UserService: 2 3 4 5 6 7 8 9 lø 11 12 13 14 15 def def getUser(self, user_id): key --- "user: :xs" user_id user = cache.get(key) if user: return user user = database. cache. set(key, user) return user setUser(se1f, user): key "user: user. id cache. delete(key) database. set(user) ](../../media/Memeory-Cache-Cache-image1.png){width="5.0in" height="3.2847222222222223in"}



![Memcached 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 cache. is a key" , cache. is a key") "this is a value" cache. set( "foo", 1, tt1-6Ø) cache. get(" foo") # wait for 60 seconds cache. get(" foo" ) null cache. set( "bar", "2") cache. get( " bar") # for some "this is a value") reason like out of memory # "bar" may be evicted by cache cache. get(" bar") null ](../../media/Memeory-Cache-Cache-image2.png){width="5.0in" height="4.215277777777778in"}



![Memcached 如 何 优 亻 匕 DB 的 查 询 cache. set( key, user) / / 此 时 database 挂 了 database.set(user) 乛 失 败 此 时 会 造 成 cache 和 database 的 数 据 不 一 致 。 导 致 下 一 次 get 的 时 候 得 到 的 是 cache 里 的 脏 数 据 。 cache.delete(key) / / 此 时 database 挂 了 database.set(user) 乛 失 败 这 种 情 况 下 ， 不 会 出 现 数 据 不 一 致 ， 只 是 cache 里 被 删 除 了 key, 下 次 需 要 重 新 ad 。 用 户 会 收 到 修 改 信 息 失 败 的 提 示 ， 用 户 自 己 可 以 选 择 再 点 一 次 " 保 存 " 进 行 重 试 。 ](../../media/Memeory-Cache-Cache-image3.png){width="5.0in" height="2.4027777777777777in"}



![Cache Aside DB Cache DB : Memcached + MySQL Web Server DB Cache ](../../media/Memeory-Cache-Cache-image4.png){width="5.0in" height="2.826388888888889in"}



![Cache Through 服 务 器 只 和 Cache 沟 通 Cache 负 责 DB 去 沟 通 ， 把 数 据 持 久 化 业 界 典 型 代 表 ． Redis （ 可 以 理 解 为 Redis 里 包 含 了 一 个 Cache 和 一 个 DB) Web Server Cache DB ](../../media/Memeory-Cache-Cache-image5.png){width="5.0in" height="2.6041666666666665in"}





what`s access pattern



**Write through cache :** everything if you want to write to the DB need write to the cache first and it is considered succeed if both write succeed.



This is really useful for applications which read the information quickly and since the same data gets written in the DB, we will have complete data consistency between cache and storage. However, write latency will be higher in this case as there are writes to 2 separate systems.



**Write around cache :** This is a caching system where write directly goes to the DB. While this ensures lower write load to the cache and faster writes,



but it will lead to a higher read latency if the a cache miss happen and we need read the data from database











**Write back cache :** just write to the cache and the write is confirmed as soon as the write to the cache completes. The cache will refresh or syncs to the DB when the cache is full. This would lead to a really quick write latency and high write throughput.



But, since the data is in the memory and not persistent. if the cache layer is died, we will lose the data

We can improve by introducing having more than one replica so that we don't lose data if just one of the replica dies.









Cache police



Normal:



before hit the database, application will check the cache quickly if the case miss, application will read from databases and update the cache



Delete or update the database



if want to delete or update the data in the database , we want to delete or update the data in the cache first, if update cost is high, just need delete the data from the cache, then delete the data from database.





80-20



we have a 80-20 rule, 20% of the tweet or photo will generate 80% read traffic which mean we can cache those popular item in the cache.





recently



base on the calculation



we can cache all tweets from all users in last 3 days



100 G is easy to fit to one cache service, we should replicate it on to multiple service to distribute the read traffic







**Which cache eviction policy would best fit our needs?**When the cache is .full, and we want to replace a link with a newer/hotter URL, how would we choose? Least Recently Used (LRU) .



Under this policy, we discard the least recently used URL first. We can use a[Linked Hash Map](https://docs.oracle.com/javase/7/docs/api/java/util/LinkedHashMap.html)or a similar data structure to store our URLs and Hashes, which will also keep track of which URLs are accessed recently.

To further increase the efficiency, we can replicate our caching servers to distribute load between them.





~~facebook cache~~





~~There is one 'leader' region and several 'slave' regions. Each region has a complete copy of the databases. There is an ongoing async replication between leader to slave(s). In each region, there are a group of machines which are 'followers',~~



~~followers will respond for the read request and cache miss will forward to leader~~

~~and the leader get the result from the their cache or query the DB~~



~~Write requests are forwarded to the leader of that region. If the current region is a slave region, the request is forwarded to the leader of that shard in the master region.~~





~~The leader sends cache-refill/invalidation messages to its followers, and to the slave leader, if the leader belongs to the master region.~~













