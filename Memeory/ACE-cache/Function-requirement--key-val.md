# Function requirement: key-value cache, support get, set, delete and support LRU-- least recently used eviction

Created: 2021-04-16 00:13:08 -0600

Modified: 2021-04-27 18:03:24 -0600

---

Function requirement: key-value cache, support get, set, delete and support LRU-- least recently used eviction



Support TTL , expiration



non-function requirement support million data, high availability, low latency





![》 ACIE 假 设 瓶 颈 机 器 数 量 存 储 资 源 1 ． 3 资 源 估 算 1M ReadQPS ， 100kWrite Q ， 单 机 支 持 1 闐 k 读 写 Key ， ue 均 为 字 符 串 ， Key 最 大 矚 v ue 最 大 1M （ 1 闐 k + 1M ） / 1 闐 k ： 11 可 根 据 应 用 调 整 ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image1.png){width="10.083333333333334in" height="5.510416666666667in"}









![D$trÔ2ŔeA @ acec4e1hŹe.nuČ.nJ. SQOÂQ CQ.chz CGe.Ak---- UDP/TQ Hosh C.A.c.ke SeoJąTS Sb 2 '23B ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image2.png){width="10.083333333333334in" height="5.427083333333333in"}



[Should we support multiple thread?]{.mark}



1.  Cache client sent a RPC call or http request to cache proxy
2.  Cache proxy will talk to zookeeper or configuration center to find out which cache service the it should contact
3.  The whole system, we use LRU strategy to remove the old data if the cache is full
4.  ~~In each service, we maintain hash table + linked list, update is constant time~~
5.  Basic cache just a key value pair in each service, if the collision happen, we can use hash table+ linked list or open address





for hash table, if there is a collision, we need a linklist to store all the collision in one slot, if the list is long, it will take time to find the key: search is O(L) l is the length of list



Or we can use ["open address"]{.mark} , there 3 option, if there is a collision, we just store the value to his next slot ( 1 position away) , if his next slot is full, we will choose the ( 2 position away, 4 position away, 8 position away)



Open addressing : each slot is key value pair, the memory can be pre allocated



For open addressing, first we calculated a slot based on the hash algorithm, if that slot has been occupied, we need to jump to 1 position away, 2 position away ..



For search, we need to search 1 position away, 2 position away ... Until we find the key value pair



If we supposed it is in the 8 position way, after we deleted ,we need to move the furthest slot or key -value pair ( assume it is in 128 position away) to 8 position -- reinsert







![Open Addressing Linear Probing Cache Performance Quadratic Probing Double Hashing Minimize Clustering ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image3.png){width="10.083333333333334in" height="6.979166666666667in"}





Linear probing is if current slot is full, use the next slot: slot next to the current slot



Quadratic probing, 1 position away , 2 position away, 4 position away, 8 positions away



Double hashing, same hash algorithm and using the last result as the input to hash again



Linear probing' s cache performance is better than others since we don't need much calculation



But double hashing will [have minimize the clustering]{.mark} compared with other 2 solution

![Chaining vs Open Addressing Load Factor Dynamic Rehashing 算 法 特 性 内 存 分 配 Delete 较 高 时 线 性 变 慢 不 必 要 平 均 分 配 Key MemoryPool 正 常 删 除 OpenAddressing 较 低 时 性 能 更 优 必 要 + 减 少 Clustering 可 预 先 分 配 特 殊 处 理 ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image4.png){width="10.083333333333334in" height="6.104166666666667in"}

Load factor = 1 mean every slot has a data, we need to add more memory space, chaining or list solution will slow when the load factor >1



Memory pool: in the memory, there are lot of discontinuous memory, we can use them as the nodes of the linked list, each node is connected by a point



![Open addressing delete • • mark as delete reinsert ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image5.png){width="10.083333333333334in" height="8.479166666666666in"}







Dynamic rehashing: we can create a new memory space, use new hash algorithm to write the data to the new memory space , only use the old hash algorithm to read

![思 考 题 如 何 在 增 加 Bucket 数 量 的 迁 移 工 作 进 行 时 维 持 服 务 ？ 创 建 新 的 内 存 空 间 将 Bucket 数 量 翻 倍 迁 移 过 程 中 使 用 两 种 哈 希 算 法 同 时 读 写 ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image6.png){width="10.083333333333334in" height="7.989583333333333in"}





How to choose:

1.  Do you want to drymatic rehashing
2.  The complexity is in memory relocate -- LinkedList or dynamic rehashing







Trade off is use a tree struct binary search tree or B+tree for index instead of hash table, the search will be logn, but tree structure is supposed range query, ( binary tree need less memory compared with hashtable)



We can just replace the long LinkedList to binary search tree



![Hash Table 优 化 策 Copy " 有 点 像 " 即 。 t 我 理 留 的 是 关 于 dynmic rehashing 地 那 ca h newchange* 地 很 难 " h up " changeW 翅 果 每 次 祁 是 完 全 新 的 庶 到 en N 9 to E 虍 我 记 得 归 ' ] h hm s 寞 坝 就 是 EST 代 替 linkedlist 》 亻 y 四 m " 開 here„. 0 使 用 BST 替 代 链 表 Rehash 增 加 Bucket 数 量 ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image7.png){width="10.083333333333334in" height="5.239583333333333in"}





![LRU vs Approx LRU 实 现 方 法 并 行 控 制 数 据 结 构 额 外 功 能 LRU 根 据 DLL 头 部 数 值 EVict 数 据 对 于 D 性 加 锁 双 向 链 表 无 Approx L,RU 随 机 读 取 多 个 Key Evict 最 老 的 数 据 无 对 于 Key 保 存 最 后 更 新 时 间 TTL Expire ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image8.png){width="10.083333333333334in" height="5.65625in"}



[Problem, need to shared a global Double linked list and lock the whole linked list. It is a bottlenecks]{.mark}



![TTL Expiration Lazy Expiration Active Expiration ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image9.png){width="10.083333333333334in" height="8.802083333333334in"}

Lazy expiration: when we read a data, if the data is expired, we just return a cache miss and delete it



Active expiration, a cron job just read the key randomly, if the data is expired, we just deleted





How to Memory allocation





![思 考 题 直 接 使 用 m 和 free 来 分 配 内 存 有 什 么 问 题 ？ Memory Fragmentation ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image10.png){width="10.083333333333334in" height="9.21875in"}

![Memcached memory PAGE PAGE SLAB 1 PAGE U 'Z/" 7/" PAGE PAGE SLAB 2 PAGE PAGE ) EMPTY ; EMPTY : EMPTY • EMPTY PAGE EMPIY , EMPTY EMPTY : EMPTY PAGE EMPTY EM EMPTY EMPTY PAGE EMPTY 'EMPTY EMPTY 'EMPTY PAGE EMPTY EMPTY EMPTY EMPTY 80 byte item 96 byte chunk 72 byte item 96 byte chunk 56 byte item % byte chunk 90 byte item 96 byte chunk 12 out of 35 MB used Page allocated to slab 1 (for chunks from 1 to 96 bytes) ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image11.png){width="10.083333333333334in" height="6.229166666666667in"}

The memory has different slab, in each slab, they have multiple pages,

[~~All page's size is 1mb~~]{.mark}

Each page they have multiple chucks



each chuck has different (chuck) size, we put the same chuck size in the same slab



All the 8bytes chuck in the slab 1

All the 10 bytes chuck in the slab 2 ( increase 20%)



The largest chuck is 1MB



![Chunk size: 80'fA(n-1) chunk chunk Classes 1: Classes 2: Classes n: Classes 40; Classes 41 : chunk chunk chunk chunk chunk chunk chunk chunk slab(1M) chunk ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image12.png){width="10.083333333333334in" height="6.635416666666667in"}



Each slab has multiple pages, all pages is connected by the point, (the pages are not necessarily contiguous) each pages has multiple chuck, slab1's page is form from 8byts chucks; slab2's page is form from 10 bytes chunk



[System will find the best slab based on the size of the key value pairs]{.mark}





![引 内 存 分 配 策 略 浪 费 / 定 Memory Fragmentation ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image13.png){width="10.083333333333334in" height="8.572916666666666in"}

![其 他 方 案 主 动 去 碎 片 化 ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image14.png){width="10.083333333333334in" height="9.21875in"}

Concurrency Control





![Allen Ning to Everyone partitionÆe Terrence Chao to Everyone 8:56 PM 8:58 PM slab pattern value size hash Allen Ning to Everyone Everyone • Type message here... 8:58 PM Last Writer Wins No Concurrency Control (Compare & Swap) Optimistic Lock Lock (Mutex) Pessimistic Lock Transaction 2-phase Commit ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image15.png){width="10.083333333333334in" height="5.4375in"}



CAS compare and swap, before write the new value, we need to read a old value first

![function cas( p: pointer to int, old: int, new: int if *p old return false return true ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image16.png){width="10.083333333333334in" height="9.385416666666666in"}



Lock the resource



![#include <pthread . count_mutex; long long count; void increment_count() count = count + 1; long long get_count() long long c; c = count; pthread_mutex_unlock (&count_mutex) ; return (c); ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image17.png){width="10.083333333333334in" height="9.90625in"}



![乐 观 锁 vs 悲 观 锁 实 现 数 据 结 构 效 果 性 能 乐 观 锁 Compare & Swap 无 如 果 两 次 读 取 不 一 致 ， 重 试 悲 观 锁 Mutex/Spin-lock bool/int 获 得 资 源 的 线 程 独 享 资 源 低 ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image18.png){width="10.083333333333334in" height="5.59375in"}





Bool/ in is indicate the resource has lock or not/ who own the lock



![Mutex vs Spin-lock 等 待 方 式 使 用 场 景 Mutex Context Switch 等 待 时 间 较 长 Spin-lock Busy Wait 等 待 时 间 较 短 ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image19.png){width="10.083333333333334in" height="5.5in"}



Spin-lock ---- while() ..... Wait the lock release





![思 考 题 如 果 需 要 实 现 悲 观 锁 ， 那 么 锁 需 要 放 在 什 么 层 面 上 ？ Bucket-level ， Shard-level ， Hash-table-level? Bucket-level 过 于 精 细 ， 内 存 消 耗 过 大 考 虑 Multi-bucket-level ， Page-level, Slab-level ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image20.png){width="10.083333333333334in" height="8.104166666666666in"}



Web service



![思 考 题 服 务 器 如 何 接 受 请 求 ？ 其 中 的 各 个 线 程 如 何 分 工 ？ Acceptor Thread + Thread p00 《 ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image21.png){width="10.083333333333334in" height="10.020833333333334in"}



The web service first has a acceptor thread to accept the request than sign the request to a thread in the tread pool



![Requests Acceptor Threads ssss Connection Queue Web Server Request Processing Threads sssss Thread Pool ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image22.png){width="10.083333333333334in" height="4.916666666666667in"}



![思 考 题 如 果 有 的 线 程 暂 时 没 有 需 要 处 理 的 请 求 ， 它 们 的 状 态 是 什 么 ？ 线 程 不 会 被 终 结 而 是 放 入 休 眠 状 态 ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image23.png){width="10.083333333333334in" height="9.427083333333334in"}



It just for read, write still use TCP

![通 讯 协 议 优 化 UDP 保 证 完 整 并 顺 序 发 送 特 点 可 能 丢 包 或 乱 序 性 能 较 低 高 无 效 递 送 处 理 ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image24.png){width="10.083333333333334in" height="5.8125in"}



![思 考 题 客 户 端 如 何 跟 分 布 式 缓 存 服 务 器 交 流 ？ 客 户 端 如 何 知 道 自 己 需 要 的 信 息 在 哪 台 机 器 上 ？ ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image25.png){width="10.083333333333334in" height="4.96875in"}



![客 户 端 与 分 布 式 服 务 器 交 互 模 式 ClientLib/Daemon Tramc Forward ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image26.png){width="10.083333333333334in" height="4.833333333333333in"}





Client lib/Daemon --- client know which service he should talk to



Proxy --- client talk to proxy and proxy know which service should talk to



Client talk to any service and service will forward the request to another service



![• ddV ddV ddV ddV ugsn AX08d ugsn ugsn Æxo.1d ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image27.png){width="10.083333333333334in" height="7.53125in"}



![Forward Ringpop Forwarding Requests: Step 1 Ringpop Forwarding Requests: Step 2 POST /123 PING ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image28.png){width="10.083333333333334in" height="4.770833333333333in"}



![思 考 题 分 布 式 缓 存 服 务 器 可 能 有 宕 机 的 ， 有 新 加 入 的 如 何 维 持 服 务 ， 分 摊 请 求 ？ Consistent Hashing Replication ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image29.png){width="10.083333333333334in" height="8.020833333333334in"}



![思 考 题 如 何 保 证 一 组 分 布 式 缓 存 服 务 器 对 每 台 负 责 的 Hash Range 的 认 识 是 一 致 的 ？ Gossip or Zookeeper ](../../media/Memeory-ACE-cache-Function-requirement--key-value-cache,-support-get,-set,-delete-and-support-LRU---least-recently-used-eviction-image30.png){width="10.083333333333334in" height="8.583333333333334in"}



Gossip --- forward the hash range information to all the nodes

Or store in the configuration service






























