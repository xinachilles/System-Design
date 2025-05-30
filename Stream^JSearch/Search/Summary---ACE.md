# Summary - ACE



---

The system has 2 part: index builder -- build the index and search or query the keyword.





When we crawler the new webpage, system will store the new webpage to database and send a kind of event to indexer service to create a inverted index for the new webpage and stored it in index store





When user search used like key word search, the search query will go to query processor service and the query process will look up the inverted index in index store and return to user







![O Ace ĹodQlÍ/t+ervie,wr. conn I Star s ](../../media/Stream^JSearch-Search-Summary---ACE-image1.png)



![4 QFS Qeg.Go•31e) C)ùagrant- extraoéivn ReseZ+ @ acecodeinterv ](../../media/Stream^JSearch-Search-Summary---ACE-image2.png)



[For indexer service]{.mark}, we run the mapreduce job to build the inverted index



The mapper job will extract the keywords from webpage



do the tokenization, normalization(vehicle= car), stemming and lemmatization





![和 分 e 仑 docs-google.com 加 resentatio 可 引 litC3B NZ 」 YBW8G82eP•wDs 、 0 ^ 《 Z V6DLbCu18 ℃ 口 hE 在 巧 巧 # ide = ． p 英 语 词 的 提 取 请 会 请 多 0 Alle 标 识 化 标 准 化 词 干 提 取 & 词 形 还 原 Tokenization Normalization Stemming & Lemmatization 词 组 词 性 姓 名 同 义 词 单 复 数 邮 箱 网 站 时 态 带 标 点 的 词 @ 爱 思 版 权 所 有 未 经 允 许 请 勿 录 制 或 传 播 acecodeinterview ℃ on ](../../media/Stream^JSearch-Search-Summary---ACE-image3.png)





The map job will read the document, split those documents into group of words, generate the key value pair and write it into the GFS

{key is keyword, value is url+position }





~~Then~~ the MapReduce framework is going to collect together every key value pair and give it to different reduce function



Reducer job will combine those key value pair together, create the [posting list]{.mark} ,store in index store



[MapReduce - ACE](onenote:Mapreduce.one#MapReduce%20-%20ACE&section-id={A28EF3D2-AD11-B849-A1F2-27F121D9A4E3}&page-id={39607F96-AA1E-8041-832A-C8E735E5BC75}&end&base-path=https://d.docs.live.net/77339d157d673f41/Documents/System%20Design/Stream,Search)



Key is keyword and value is a list of {url+position } pair

Keyword ->url+position





![1.5 Indexer Sarcla @ acecoclQ-iA±ervi2AA. extra VRC, Peclucex ](../../media/Stream^JSearch-Search-Summary---ACE-image4.png)





From crawl the new page to build index and able to be searched by user, it will take [1-2 days]{.mark}





[How to store (or shard) the posting list, inverted index :]{.mark}

Termed based or document based: Document id ( stored the web page as document)





![Document- Partitioned Index Term- Partitioned Index Advantages Posting Lists are Smaller Easy to manage and scale Intersections are local to a machine Index updates can be done dynamically and one partition at a time Only a subset of machines are used for each query Disadvantages All machines are contacted for each query Merging results from all machines can be expensive Network traffic to all machines can be expensive Remote intersections can be expensive as entire posting lists need to be copied over network Index updates need to be done for entire index ](../../media/Stream^JSearch-Search-Summary---ACE-image5.png)



















































[Stored the document partition inverted in the memory]{.mark}

[Also stored the long posting list on the disk by using the term partitioned]{.mark}







[老师你好， 在 google search 的 index 设计里，你提到index partition 的策略是hybrid]{.mark}

[我的理解是 index 是用 document parittion ，存放在内存里，如果内存里存不下，在把剩余的存在硬盘里，用的是term-partition。不知道这样的理解对不对？]{.mark}



基本正确，就提一点。

内存部分会使用分布式缓存，所以使用 document partition 之后没有绝对的存不下的问题。我们说不在内存中存储所有的 Inverted Index 数据是因为不常用的长尾 index 放在内存里意义不大，太贵。这些数据就只放在硬盘里就好



一方面，在多数情况下，对于常用的索引，也就是每个关键词中排序最靠前的内容的 Posting List（可以是词频最高 (term frequency)，也可以是所在网站权重更高），我们保留一定数量的在内存中。另一方面，对于很少被查询的内容，我们可以将这些 Posting List 放在硬盘中，在少数情况下使用。





![Doc ID Posting List Term Frequency Impact ](../../media/Stream^JSearch-Search-Summary---ACE-image6.png)





In each posting list, we sort the doc base on the doc id; or we also can consider to sort the doc base on the term frequency ( if the key word occurs more often in one doc, we put that doc on the first)



[Stored based on the doc id]{.mark} --- for keyword intersection





[Index maintainer service for new index]{.mark}



![硬 盘 索 引 更 新 模 式 0 重 建 Rebuild 间 断 合 并 Intermittent Merge 渐 进 更 新 Incremental Update ](../../media/Stream^JSearch-Search-Summary---ACE-image7.png)

[Intermitted merge -- build the new posting list n other place then merge together ( better one)]{.mark}



[Incremental update -- when user just search a key words, update this key words at the same time]{.mark}

![功 能 性 需 求 非 功 能 性 需 求 非 需 求 1.2 理 解 需 求 通 过 关 键 词 快 速 及 时 地 找 到 相 关 信 息 高 扩 展 性 ， 支 持 全 网 索 引 对 于 核 心 网 站 ， 低 延 迟 索 引 对 于 用 户 ， 低 延 迟 返 回 搜 索 结 果 高 可 靠 性 ， 返 回 内 容 与 关 键 词 相 关 非 英 语 支 持 ， 爬 虫 ， 搜 索 结 果 排 序 ](../../media/Stream^JSearch-Search-Summary---ACE-image8.png)



![单 一 关 键 词 搜 索 查 询 结 果 LAU 缓 存 内 存 索 引 2 ． a. 智 能 排 序 Posting List b. Doc ID 扫 《 序 Posting List 硬 盘 索 引 3 ， a. 内 存 中 尚 未 合 并 的 索 引 b. 智 能 排 序 Posting List c. Doc ID 扌 非 序 Posting List ](../../media/Stream^JSearch-Search-Summary---ACE-image9.png)

[We can temporarily store the result of each search in memory]{.mark}



[For each query, system will look at the cache first, then query the in meory index storage]{.mark}







[For each term, there are more than one posting list]{.mark}

[One is sorted base on the impact and frequency]{.mark}

[One is sorted base on the doc id]{.mark}



[First 2- 3 page will return the result from impacted or frequency sorting posting list]{.mark}

[Then return from doc id sorting posting list]{.mark}

[Then return from the posting list from disk partition by term base]{.mark}



<https://forum.acecodeinterview.com/t/topic/190/2>

LRU= frequent query cache

![Posting List Keyword [(doc_id_l, pos_l, pos_2), (doc_id_2, pos_3), ...l ](../../media/Stream^JSearch-Search-Summary---ACE-image10.png)



![思 考 题 五 Posting List 存 在 内 存 还 是 硬 盘 ？ 多 数 放 内 存 常 用 放 内 存 扌 番 acecodeinterview.com ](../../media/Stream^JSearch-Search-Summary---ACE-image11.png)

[Document partition index:]{.mark}

Pros:

1.  Posting list are smaller and could be short and easy to update (update in one partition)
2.  Intersection locally



Cons:

For each key work, need to scan all the machine, network traffic to all the machine could be expensive



[Term partition index:]{.mark}





Pros:

Don't need scan all the machine

Cons

Term based will have hot words

Need a additional machine for intersection

Expensive for index update

Cannot store in memory

![BigTable Tablet Read/Write memtable Memory GFS tablet log Write Op Read Op SSTab1e Files Figure 5: Tablet Representation ](../../media/Stream^JSearch-Search-Summary---ACE-image12.png)

indexer Index maintainer + on disk index storage = big table



Write operation first write a log then write to memory first



Mem table is ~~line indexer~~ Index maintainer for new index

Then merger into disk



SStable is for disk index



![D sk ](../../media/Stream^JSearch-Search-Summary---ACE-image13.png)



Index builder will first update the in-memory storage,

Then update the index maintainer --

Index maintainer will have all new posting list

Then it will fresh into the disk periodically





![索 引 的 内 存 数 据 结 构 Linked List of Singly Linked List Variable Length Array Fixed_size Array ](../../media/Stream^JSearch-Search-Summary---ACE-image14.png)



Linked list we need store the point information

Variable length array -- the array can be extend but cannot insert new data quickly

Linked list fixed size array -- multiple fixed size array linked with array



![索 引 的 硬 盘 数 据 结 构 - Gap Variable Byte Encoding ](../../media/Stream^JSearch-Search-Summary---ACE-image15.png)

Doc gap: store the first doc id then rest just store the difference between previous one and current one (docid2 - docid1)



Variable byte encoding, store the number and indicate how many bytes for the number, small number just need less bytes



We can use doc gap and variable bytes encoding at same time



![思 考 题 六 多 关 键 词 搜 索 (AND) 使 用 什 么 算 法 ？ 双 指 针 (Two Pointer) 跳 表 (Skip List) ](../../media/Stream^JSearch-Search-Summary---ACE-image16.png)![思 考 题 二 如 何 通 过 索 引 来 实 现 多 个 单 词 的 搜 索 ？ 对 于 Posting List 取 交 集 ](../../media/Stream^JSearch-Search-Summary---ACE-image17.png)



![思 考 题 七 关 键 词 组 搜 索 怎 么 实 现 ？ 利 用 位 置 信 息 关 键 词 组 索 引 ](../../media/Stream^JSearch-Search-Summary---ACE-image18.png)![田 考 题 三 如 何 进 行 索 引 来 实 现 连 续 的 词 组 搜 索 ？ Posting List 存 入 位 置 信 息 特 定 词 作 为 整 体 索 引 (e.g. New York, Doctor Who) ](../../media/Stream^JSearch-Search-Summary---ACE-image19.png)



If the position value is continue ... New (postion 1) york (position 2)



Or build index for "New York" , "new York "is the keyword



API:

![• euempueqoe „ asuodsay // 139 *sanbay // ](../../media/Stream^JSearch-Search-Summary---ACE-image20.png)

Start = 30 : start 3th page (each page has 10 item) ( index update is not very frequency)

![Instagram GET /vl/feed? image_count={image count}& last_timestamp={ts} ](../../media/Stream^JSearch-Search-Summary---ACE-image21.png)![1.10 MapReduce Bigtable, Document-based Partition, Cache ](../../media/Stream^JSearch-Search-Summary---ACE-image22.png)![服 务 器 存 储 L11 容 灾 设 计 MapReduce Persistency Master-slave Replication (GFS ， DB ， Cache) ](../../media/Stream^JSearch-Search-Summary---ACE-image23.png)

Mapreduce will store the result in GFS



![Mapper/Reducer Ratio 1.12 Index Freshness DB Load Cache Hit Rate ](../../media/Stream^JSearch-Search-Summary---ACE-image24.png)

Index freshness:

For those sites that are updated frequently

We need to check if we update the index for those website



![1 · 13 Twitter Hashtag Search vs Google Search 逆 向 索 引 存 储 排 序 因 素 Hashtag Search 仅 Hashtag 内 存 作 者 +retweet Google Search 提 取 全 文 单 词 内 存 + 硬 盘 ](../../media/Stream^JSearch-Search-Summary---ACE-image25.png)

























