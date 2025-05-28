# Spotify top 10 

Created: 2021-04-15 10:35:54 -0600

Modified: 2021-05-25 16:42:06 -0600

---

Function requirement:



1.  Top 10 most popular song in last 7 days
2.  Real time update or maybe delay 1-2 minutes





Non function requirement



Support million of songs

High availability

does not require absolute accuracy





![2 ． 假 设 瓶 颈 3 资 源 估 算 每 秒 1M 次 歌 曲 播 放 每 秒 1 次 歌 单 读 取 歌 曲 播 放 EventQPS david lu to Everyone 归 并 p90 的 时 候 要 考 虑 不 P W to Everyone 其 实 p 那 边 我 比 较 好 奇 1 做 的 " 就是类 似 peakho dawn leave to Everyone 它 统 计 的 就 是 每 个 bucket 不 一 样 啊 Everyone ， Type message here.. 、 ](../../media/Steam^JCollection-TopK-Spotify-top-10-image1.png){width="5.0in" height="2.4722222222222223in"}



The system should have four part: ( all the data collection system)



1.  Data collection
2.  ~~Data transfer~~
3.  ~~Data store~~ top 10 calculation
4.  Data fetch or reading



![数 据 流 事 件 采 集 事 件 整 合 传 递 事 件 存 储 P W to Everyone 其 实 p90 那 边 我 比 做 的 " 就 是 类 似 pe dawn leave to E 它 计 的 就 是 每 个 b 不 一 样 啊 P W to Everyone 是 按 时 间 段 分 bukc 《 Everyone · Type message here.. 事 件 读 取 ](../../media/Steam^JCollection-TopK-Spotify-top-10-image2.png){width="5.0in" height="2.2777777777777777in"}

1.  Data collection : when user play a music, at the same time, in the "music service ", system will [remeber]{.mark} this music has been played once at certain time,
2.  this play event should be sent to kafka queue



![출, (弋 -4 ](../../media/Steam^JCollection-TopK-Spotify-top-10-image3.png){width="5.0in" height="2.1875in"}



There two options 1. with Kafka, the benefit is we will store another event data [as a replica in Kafka]{.mark}, we can control the data transfer speed from music service to topk generator

But we will get like 100ms delay if we used Kafka





Or we can ask music service write all information (send request) to Top k generator service directly if we have the low latency requirement



[Top k generator will generate the top k information in cache]{.mark}





![型 qe. ト よ の 工 を 名 三 6 の 、 の 」 コ 00 コ で ・ メ u コ ョ 00 、 ! を ・ こ 2 ](../../media/Steam^JCollection-TopK-Spotify-top-10-image4.png){width="5.0in" height="3.9722222222222223in"}

For top k cache we can use LFU cache -- least frequently used



LFU -- Hashmap +linkedlist, in the list each node is for the count ( all music played with 1000 times)

Hashmap is mapping with music id -> node



There two stream (in Kafka (two point in kafka) or additional database) to update the counter system

1.  New song just played.
2.  Old song player from 7 days ago



Sharding by song id or song name





O(k) find the top 10



Way 2: hashmap + heap : cannot update the song is the song is out of the 7 days windows



Way 2 count -min sketch instead of hashmap





![思 考 题 如 果 单 机 存 不 下 数 据 怎 么 办 ？ 分 布 式 每 台 机 器 负 责 某 一 部 分 歌 曲 结 果 再 做 一 轮 整 合 ](../../media/Steam^JCollection-TopK-Spotify-top-10-image5.png){width="5.0in" height="5.201388888888889in"}





![如 果 歌 单 每 天 更 新 一 次 ， 选 择 Stream Processing 还 是 MapReduce? 根 据 实 时 性 要 求 ， 选 择 MapReduce ](../../media/Steam^JCollection-TopK-Spotify-top-10-image6.png){width="5.0in" height="3.201388888888889in"}



Mapreduce is good for the application latency > 1 hour









![High-level architecture API Gateway Storage Distributed Messaging System fast path slow path Distributed Messaging Partitioner Partition Distributed File System Frequency Count Top K MapReduce Job MapReduce Job Aggregates in-rnernory over the course of several minutes. Generates files of the specified size. ](../../media/Steam^JCollection-TopK-Spotify-top-10-image7.png){width="5.0in" height="2.5347222222222223in"}



Tradeoff



1.  If need real time -- stream process vs mapreduce
2.  If need accuracy -- hashamap vs count-min sketch
3.  How to read the data -- mapreduce vs stream process





![Tradeoff 总 结 Can Gao to Everyon double linkedlist Allen Ning to Everyc 没 太 懂 'Eüshard 飠 linkedListsH%ti# Terrence Chao to E' 按 歉 shard 不 是 也 dawn leave to Even 感 觉 薇 博 新 闻 那 些 力 realtime 的 Everyone Type message here. 实 时 性 要 求 可 靠 性 要 求 数 据 读 取 灵 活 性 ](../../media/Steam^JCollection-TopK-Spotify-top-10-image8.png){width="5.0in" height="2.8680555555555554in"}










