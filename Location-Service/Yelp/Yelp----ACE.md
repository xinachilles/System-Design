# Yelp -- ACE



---

1.  [requirement]{.mark}



Service will store the information about restaurants and user can search the restaurants base on the name, type...

them and service will return the result base on user`s location

Functional Requirements:

1. Users should be able to add/delete/update Places.

2. Given their location (longitude/latitude), users should be able to find all nearby places within a given radius.

3. Users should be able to add feedback/review about a place. The feedback can have pictures, text, and a rating.

non- function requirement:

1.  support million restaurants .. and billion user
2.  high availability - user can visit anytime





![CACIIt 功 能 性 需 求 非 功 能 性 需 求 2 · 2 理 解 需 求 根 据 地 图 范 围 以 及 餐 厅 名 称 / 类 型 / 特 征 / 菜 品 查 询 餐 厅 上 传 或 者 浏 览 餐 厅 评 论 以 及 打 分 上 传 或 浏 览 餐 厅 ／ 菜 品 图 片 高 扩 展 性 ， 支 持 百 万 级 餐 厅 以 及 亿 级 用 户 高 可 用 性 ， 服 务 随 时 可 以 访 问 0 爱 思 版 权 所 有 未 经 允 许 请 勿 录 制 或 传 指 acecodeinterviev ](../../media/Location-Service-Yelp-Yelp----ACE-image1.png)









![C 《 假 设 瓶 颈 服 务 资 源 存 储 资 源 2 · 3 资 源 估 算 IOOMMAU 平 均 每 人 每 月 进 行 10 次 搜 索 1M 商 户 每 个 商 户 平 均 存 槠 思 条 评 论 / 5 闐 张 照 片 搜 索 Q 商 家 数 据 存 储 搜 索 以 闐 M * 10 / （ * 24 女 36 闐 ） 一 385Q 商 家 数 据 一 1M 女 （ 皿 与 KB + 500 * 2 、 1B ） 、 = IPB ](../../media/Location-Service-Yelp-Yelp----ACE-image2.png)



1200 * 10/30 = 400 ( 1M = 12)



2.  [service]{.mark}



![2 · 4 系 统 设 计 图 商 户 索 引 商 户 搜 索 评 价 打 分 服 务 ](../../media/Location-Service-Yelp-Yelp----ACE-image3.png)





![巨 一 0 爱 思 版 权 所 有 未 经 允 许 请 勿 录 制 或 传 指 acecodeinterview.com ](../../media/Location-Service-Yelp-Yelp----ACE-image4.png)



3.  data follow

the system has 3 part :

1.  restaurants owner can create and update the restaurants information ( [business profile services]{.mark}) and system will build the index [(indexer service)]{.mark} for those restaurants and update to index DB or use elasticsearch
2.  search the restaurants ( search service)
3.  review and comments (reviewed service)



business media service will store restaurants picture

![商 户 索 引 Geohash 索 引 商 户 地 址 类 型 索 引 商 户 类 型 餐 厅 类 型 范 围 索 引 营 业 时 间 关 键 词 索 引 店 名 菜 单 ](../../media/Location-Service-Yelp-Yelp----ACE-image5.png)



sharding base on the Geohash or if we use google S2, we can sharding base on the s2 cell id



[S2 cell is like quardtree it decode the earth into small cell ( one cell could be one city or one county)]{.mark}



[And every cell has a number , it index by "Hilbert curve", if two cell are next to each other, their index also should be close]{.mark}



according to the traffic pattern, active user count, user query count in one hour.. we can precompute sharding strategy

like s2 cell id 1 -10 into one sharding ..



the we know how to sharding the user base on user's geohash

from 1111-2222 go to service

if use elasticsearch -- don't need indexer service



![Business Profile Table lanqing yang to Everyone EATSfiE! Allen Ning to Everyone Google Dong Li to Everyone To: Everyone I Type message here... Business Business ID Name S2 Cell ID Address ](../../media/Location-Service-Yelp-Yelp----ACE-image6.png)



s2 cell ID is sharding key



![Clit Business Media Table Media ID Business ID Partition/Shard Key Media URL Review ID ](../../media/Location-Service-Yelp-Yelp----ACE-image7.png)



if the thumbnail need to be shown in the search, we need to sharding base on the cell id



![Review Table Review ID Business ID Partition/Shard Key Content Author ID ](../../media/Location-Service-Yelp-Yelp----ACE-image8.png)



![Elasticsearch Document Store Search Engine ](../../media/Location-Service-Yelp-Yelp----ACE-image9.png)

flexible schema

[elasticsearch will build the index base on the column]{.mark}

![商 户 索 引 Geohash 索 引 商 户 地 址 类 型 索 引 商 户 类 型 餐 厅 类 型 范 围 索 引 营 业 时 间 Google S Dong Li to 凡 是 涉 及 ！ Allen Ning 如 果 要 用 飞 某 一 个 bu id or nam 丨 Type mess. 关 键 词 索 引 店 名 菜 单 ](../../media/Location-Service-Yelp-Yelp----ACE-image10.png)![执 行 搜 索 01 找 到 位 置 相 关 Shards 02 找 到 范 围 内 的 商 户 03 根 据 类 型 / 范 围 / 关 键 词 筛 选 04 根 据 距 离 / 评 分 / 匹 配 度 排 序 1 ](../../media/Location-Service-Yelp-Yelp----ACE-image11.png)



3.  Data flow:

















API

![](../../media/Location-Service-Yelp-Yelp----ACE-image12.png)![](../../media/Location-Service-Yelp-Yelp----ACE-image13.png)

last_review_id...













