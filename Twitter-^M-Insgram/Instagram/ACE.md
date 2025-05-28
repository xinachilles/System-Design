# ACE

Created: 2021-04-26 14:46:21 -0600

Modified: 2021-06-02 17:46:51 -0600

---

![fekch fen J BA-.1 C..ZĂ- vvplocd serv ICL cos S SOJI ](../../media/Twitter-^M-Insgram-Instagram-ACE-image1.png){width="5.0in" height="3.5in"}



![كمار ليا هم) لمم phe+•o لهههد طم ي 4ما هلصط محدامن مد علمه *to table اعم م مهى كه yt•c عاه له6 معما عوم -*-fahou ك c..che والب crdkt Qce-coAe:q•c*Q.aRevJ . ](../../media/Twitter-^M-Insgram-Instagram-ACE-image2.png){width="5.0in" height="2.4027777777777777in"}









**Functional Requirements**

Users should be able to upload/download/view photos.



Users can perform searches **based on** photo/video titles.



Users can follow other users.



The system should be able to generate and display a user's timeline consisting of top photos from all the people the user follows.

![假 设 瓶 颈 服 务 资 源 存 储 资 源 3 · 3 资 源 估 算 IBDAU ， 5 年 存 储 ， 1M 照 片 10 天 发 照 片 1 次 ， 1 天 读 取 信 息 流 10 次 Read QPS * 10 / （ 3 * 24 ） 、 Read 一 1 ， ， ， = 1 ， QPS Write 一 1 ， ， ， / （ 10 * 36 闐 * 24 ） 、 = 1 ， QPS 1 ， ， ， 1M * 365 * 5 / 10 、 = 183 PB ](../../media/Twitter-^M-Insgram-Instagram-ACE-image3.png){width="5.0in" height="2.9375in"}





[Is a read heavy system]{.mark}



[Peak QPS * 5 or * 10]{.mark}



Two different type of user normal user -- few followings

And follow a lot of other users



Celebrity --- a lot of followings





![： 宀 艹 " 里 模 式 有 最 多 Follower 的 人 的 Follower 数 量 Following 最 多 人 的 人 的 № № w № g 数 量 ](../../media/Twitter-^M-Insgram-Instagram-ACE-image4.png){width="5.0in" height="3.951388888888889in"}



![pwsh/eul.l cre-4iet pst fable async 7 fbllawe.c se.'Jd(-L t JDI* ](../../media/Twitter-^M-Insgram-Instagram-ACE-image5.png){width="5.0in" height="2.2916666666666665in"}



Follower table is who user follower



Following table is who follow me/user



[We need to store data about users, their uploaded photos, and people they follow]{.mark}



![Post Table Post ID Author ID Partition/Shard Key Create Time * optional Image URL Is Celebrity Post* ](../../media/Twitter-^M-Insgram-Instagram-ACE-image6.png){width="5.0in" height="2.7083333333333335in"}







![Feed Table Primary Key -> (User ID, Create Time, Post ID) User ID Author Author ID Name Author Image URL Post ID Post Image Clustering Key Create Time Partition/Shard Key ](../../media/Twitter-^M-Insgram-Instagram-ACE-image7.png){width="5.0in" height="2.6666666666666665in"}



Sort by create time and post id



![Follow Table User ID Following User ID User ID c Follower User ID A is following B (Used for Pull) D follows C (Used for Push) ](../../media/Twitter-^M-Insgram-Instagram-ACE-image8.png){width="5.0in" height="2.6944444444444446in"}



![User Table User ID Partition/Shard Key User Name Profile Join Time Photo URL ](../../media/Twitter-^M-Insgram-Instagram-ACE-image9.png){width="5.0in" height="2.6041666666666665in"}





![思 考 题 八 缓 存 的 应 用 场 景 ？ Query Cache 用 户 访 问 信 息 流 时 预 加 载 ](../../media/Twitter-^M-Insgram-Instagram-ACE-image10.png){width="5.0in" height="6.395833333333333in"}



For pagination



[CDN -- application will check the CDN first if not then get the photo from S3 base on the URL]{.mark}





![POST /vl/images POST /vl/posts ' image_url" : image_url, "description": "hello world" ](../../media/Twitter-^M-Insgram-Instagram-ACE-image11.png){width="5.0in" height="4.256944444444445in"}



Post image will get a url

![GET /vl/feed? image_count={image_count}& last _ timestamp={ts} ](../../media/Twitter-^M-Insgram-Instagram-ACE-image12.png){width="5.0in" height="2.9722222222222223in"}



Pagination ---

Ask service return all the photo relatively new than last_timestamp



Select *

From time> last_timestamp





Assume the photo sort by time and olderest one will on the top

Non functional requirement

Allow latency for posting the photo



![功 能 性 非 功 能 性 需 求 非 需 求 3 · 2 理 解 需 求 Instagram 上 传 照 片 读 取 信 息 流 高 可 用 性 读 取 信 息 流 低 延 迟 上 传 到 进 入 信 息 流 允 许 一 定 延 迟 follow/unfollow ， Feed Ranking ](../../media/Twitter-^M-Insgram-Instagram-ACE-image13.png){width="5.0in" height="3.0069444444444446in"}![非 功 能 性 需 求 ili 呼 扩 展 性 Reliability 可 靠 性 （ 正 确 性 ） Availability 可 用 性 Consistency 一 致 性 Latency 延 迟 Efficiency 效 率 ](../../media/Twitter-^M-Insgram-Instagram-ACE-image14.png){width="5.0in" height="2.4166666666666665in"}

Reliability -- if we can save the data correctly or return the correct result to user, all user have the same source of truth

Reliability --Auto complete system -- must return top 10 or can return any 10 result from top 20 or top 50



Consistency - all the user can read the same result (may be not correct result -- reliability)



![Blob Storage Image URL Image Data ](../../media/Twitter-^M-Insgram-Instagram-ACE-image15.png){width="3.8541666666666665in" height="3.2847222222222223in"}



Demonized,



We don't need to join the post table to get the post information again



URL is S3 URL



The real photo will stored in the bob storage like[HDFS](https://en.wikipedia.org/wiki/Apache_Hadoop)or[S3](https://en.wikipedia.org/wiki/Amazon_S3).



But if user rename, the data will be inconsistence



A is celebrity , B is list of user follow A



If C send a picture , all the user follow C will get push (like D)



D follow C, D is follower



![1 月 13 日 问 题 一 · Follower table 和 Following table 都 包 含 完 整 的 用 户 与 用 户 之 间 的 Follow 关 系 。 跟 使 用 场 景 ， 也 就 是 你 说 的 发 布 者 还 是 读 取 者 无 关 。 · Celebrity Post Table 之 所 以 要 和 Following Table 一 起 使 用 ， 是 因 为 读 取 用 户 Feed 的 时 候 ， 需 要 先 找 到 这 个 用 户 都 关 注 了 谁 ， 再 用 这 个 关 注 列 表 去 Celebrity Post Table 里 读 被 关 注 者 的 照 片 。 ](../../media/Twitter-^M-Insgram-Instagram-ACE-image16.png){width="5.0in" height="1.2152777777777777in"}



There are two types of cache:

Query cache : if user query the database, we can store the result in cache, web application may only load the first 10 page, if user want to more information ( next 10 page), web application can fetch the data from the cache

General purpose cache : for popular data



scalability

![3.8 Load Balancer, Stateless Service, Async Worker Cache, CDN, DB Sharding, Consistent Hashing ](../../media/Twitter-^M-Insgram-Instagram-ACE-image17.png){width="5.0in" height="2.5972222222222223in"}



![3.9 GRi±it Load Balancer, Multiple Machines/Datacenters Multiple Cache/DB, Consistent Hashing, Master-slave Replication ](../../media/Twitter-^M-Insgram-Instagram-ACE-image18.png){width="5.0in" height="2.3402777777777777in"}

High availability



We can use consistent hashing to back up data on the previous node



![Feed ops 3.10 Feed Latency Availability Async Load Cache Hit Rate DB Load ](../../media/Twitter-^M-Insgram-Instagram-ACE-image19.png){width="5.0in" height="2.0277777777777777in"}



Feed latency-- how long the system can push the new post to feed table



Availability -- all service availability

Async load -- how long the async job backlog



DB load ( database have enough storage)



















