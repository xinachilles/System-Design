# Scale



---

We can cache URLs that are frequently accessed. We can use Memcache, that store full URLs with their respective hashes. The application servers, before hitting backend storage, can quickly check if the cache has desired URL.





**How much cache should we have?**We can start with 20% of daily traffic and based on clients' usage pattern we can adjust how many cache servers we need.



As estimated above we need 4G memory( [Scenario](onenote:#Scenario&section-id={5C9BC554-D710-BB4A-B564-D4493A1F1645}&page-id={E2B39DFD-80FC-B247-81D1-CA95E38CBC76}&end&base-path=https://d.docs.live.net/77339d157d673f41/Documents/9%20chapter/System%20Design%20and%20OO%20Design/TinyURL.one) ) to cache 20% of daily traffic



since a modern day server can have 256GB memory, we can easily fit all the cache into one machine, or we can choose to use a couple of smaller servers to store all these hot URLs.



**Which cache eviction policy would best fit our needs?**When the cache is .full, and we want to replace a link with a newer/hotter URL, how would we choose? Least Recently Used (LRU) can be a reasonable policy for our system. Under this policy, we discard the least recently used URL first. We can use a[Linked Hash Map](https://docs.oracle.com/javase/7/docs/api/java/util/LinkedHashMap.html)or a similar data structure to store our URLs and Hashes, which will also keep track of which URLs are accessed recently.

To further increase the efficiency, we can replicate our caching servers to distribute load between them.



**How can each cache replica be updated?**Whenever there is a cache miss, our servers would be hitting backend database. Whenever this happens, we can update the cache and pass the new entry to all the cache replicas. Each replica can update their cache by adding the new entry. If a replica already has that entry, it can simply ignore it.

![a shortened Yes, m error 401 NO, redirect •al U RL as URL expired or user does not have pernissions URL nd orignal U RL not gnal URL L not found. URL fou Database update cache Request flow for accessing a shortened URL ](../../media/TinyURL^MID-gen-TinyURL-Scale-image1.png)



![us DNS DNS Web Server Shared DB Web Server Memcached Memcached ](../../media/TinyURL^MID-gen-TinyURL-Scale-image2.png)![Scale 扩 展 SHORT KEY · 如 果 最 开 始 ， short key 为 6 位 ， 下 面 为 sho 戊 key 增 加 1 位 前 置 位 · AB 1234 乛 CAB 1234 · 还 有 一 种 做 法 是 把 第 1 位 单 独 留 出 来 做 sharding key, 总 共 还 是 6 位 · 该 前 置 位 的 值 由 Hash(long_url) ％ 62 得 到 · 该 前 置 位 则 为 sharding key · Consistent Hashing · 相 关 练 习 h № 丿 ／ 、 、 、 lintcode.com/en/problem/consistent-hashing/ · 将 环 分 为 62 个 区 间 · 每 台 机 器 在 环 上 负 责 一 段 区 间 ](../../media/TinyURL^MID-gen-TinyURL-Scale-image3.png)

0-61 then map to 62`s characters

![](../../media/TinyURL^MID-gen-TinyURL-Scale-image4.png)



![USA CN DNS Shared DB DNS Web Server Shared DB Shared DB Memcached Shared DB Memcached Consistent Hashing fi Mapping Web Server -E , hard code Web Server ](../../media/TinyURL^MID-gen-TinyURL-Scale-image5.png)



![](../../media/TinyURL^MID-gen-TinyURL-Scale-image6.png)










