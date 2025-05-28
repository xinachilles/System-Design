# Summary -ACE

Created: 2021-04-04 16:17:29 -0600

Modified: 2021-05-29 23:32:57 -0600

---

![nans -4 ĂoqdaJd ](../../media/File-System-Drop-box-Summary--ACE-image1.png){width="5.0in" height="2.9375in"}

Function: shared the folder between different device

shared the folder between users

upload, download, update and delete the file

able to see the different version of the file





Non-function requirement:



Scalability - support billion of users



Efficiency - File storage and updates take up as little bandwidth and storage space as possible

Don't need to save the duplicate file



Eventual Consistency - keep consistency between different devices



![2 · 3 资 源 估 算 IOOMDAU 每 人 每 天 平 均 上 传 1 个 新 文 件 ， 更 新 10 个 旧 文 件 假 设 文 件 平 均 10 MB Ongoing Connection 瓶 颈 更 新 QPS 数 据 存 储 ](../../media/File-System-Drop-box-Summary--ACE-image2.png){width="5.0in" height="2.5416666666666665in"}



![2 · 3 资 源 估 算 (cont.) Ongoing Connection 一 1 闐 M * ％ 、 ： 50M 服 务 资 源 更 新 QPS 1 M * 10 / （ 24 * 36 佣 ） 一 12k QPS 上 传 QPS 1 佣 M / （ 24 * 36 ） 一 1.2k QPS 存 资 5 年 存 储 一 1 闐 M 女 IOMB 365 * 5 、 = 18 PB ](../../media/File-System-Drop-box-Summary--ACE-image3.png){width="5.0in" height="2.6041666666666665in"}



![文 件 存 储 服 务 核 心 子 系 统 元 数 据 服 务 通 知 服 务 ](../../media/File-System-Drop-box-Summary--ACE-image4.png){width="5.0in" height="3.0277777777777777in"}

1.  File storage , blob storage, chuck service
2.  Metadata
3.  Notification









Blob storage : hash - > block



![文 件 存 储 服 务 抽 象 (hash, block) SHA-256 Max 4 、 1B Chunk ](../../media/File-System-Drop-box-Summary--ACE-image5.png){width="5.0in" height="3.6805555555555554in"}



![田 考 题 文 件 更 新 时 应 该 如 何 存 储 ？ 改 动 较 大 ． > 存 储 完 整 文 件 改 动 较 小 · > 存 储 Diff ](../../media/File-System-Drop-box-Summary--ACE-image6.png){width="5.0in" height="5.361111111111111in"}

![Content-addressable Storage (hash, block) SHA-256 Max4MBChunk ](../../media/File-System-Drop-box-Summary--ACE-image7.png){width="5.0in" height="2.7777777777777777in"}



The address is base on the content



That means uses hash to find the file, system will first compare the hashes before each upload, and once there is a match, it means that the block already exists and there is no need to upload this block



The file will be cut into different block, and each block has a hash



![bl (4MB) SHA-256 video.avi (14MB) b2 (4MB) SHA-256 b4 b3 (4MB) (2MB) SHA-256 SHA-256 h3 ](../../media/File-System-Drop-box-Summary--ACE-image8.png){width="5.0in" height="2.2291666666666665in"}



Metadata service



![文 件 分 段 上 传 ， 分 段 存 储 有 什 么 好 处 ？ 上 传 成 功 率 最 大 程 度 避 免 反 复 存 储 改 动 文 件 时 只 需 要 更 新 少 数 分 段 ](../../media/File-System-Drop-box-Summary--ACE-image9.png){width="5.0in" height="3.486111111111111in"}



File history --- file journal -- like change log, each change has different journal id



![File Journal Table Key -> (Namespace ID, Journal ID) Namespace ID Relative Path Journal ID Blocklist ](../../media/File-System-Drop-box-Summary--ACE-image10.png){width="5.0in" height="2.3402777777777777in"}

Journal id will auto increase, like a version number



Block list store the list of hash



5.2 用户数据库

| **USER ID** | **USERNAME** | **EMAIL** | **NAME** | **DEVICE NAME** |
|-------------|--------------|-----------|----------|-----------------|





![Namespace Table Namespace ID Namespace Type Owner ID ](../../media/File-System-Drop-box-Summary--ACE-image11.png){width="5.0in" height="2.3194444444444446in"}

If namespace is a shared type --- list of owner

Like file id





![Blob Storage SHA-256 Hash Block ](../../media/File-System-Drop-box-Summary--ACE-image12.png){width="5.0in" height="2.6458333333333335in"}

upload file:





![Metaservel Client commit(nsid=l, "Ivideo.avi", ](../../media/File-System-Drop-box-Summary--ACE-image13.png){width="5.0in" height="3.5277777777777777in"}



1.  client talk to meta data service said: I want to upload video.avi and the hash list is "h1, h2,h3,h4 " and name space id = 1 (file id = 1)
2.  Meta service will check if service already have h1.... H4, if no send a new block -- nb

![Client h2), [bl, b21) vow' store([h3, h4], [b3, b4]) ](../../media/File-System-Drop-box-Summary--ACE-image14.png){width="5.0in" height="5.048611111111111in"}



Then client will talk to [block service]{.mark} and store h1--b1, h2---b2 , h3--b3, h4-b4 to block server



Then client talk to mataservce and mata server give client a journal id = 15



![L Client commit(nsid=l, "Ivideo.avi", ](../../media/File-System-Drop-box-Summary--ACE-image15.png){width="5.0in" height="3.7083333333333335in"}



[Download service]{.mark}



![• "'Vtaserver DL Client list(nsids_and_jids={l: 14, 2: 29)}) Cos. ](../../media/File-System-Drop-box-Summary--ACE-image16.png){width="5.0in" height="3.8958333333333335in"}

Client fist take to meta server: the current version of file is namespace 1,journal id 14 ; namespace 2, journal id 29



Meta data find namespace 1 has new update h1,h2,h3,h4

![, h2J) retrieve([h3, h4]) ](../../media/File-System-Drop-box-Summary--ACE-image17.png){width="5.0in" height="5.354166666666667in"}



Then client talk to block server to fetch the new update h1,,, h4

![通 知 服 务 通 过 Websocket 通 知 需 要 进 行 同 步 的 客 户 端 ](../../media/File-System-Drop-box-Summary--ACE-image18.png){width="5.0in" height="3.9722222222222223in"}





When metadata service found new update. It will ping client via WebSocket



![](../../media/File-System-Drop-box-Summary--ACE-image19.png){width="5.0in" height="3.1527777777777777in"}



When the UL client write the new data to block server and meta server, meta data server will ping another client --- DL client ( right picture), don't need to wait all writing finish



High level

![ÄaqdaJd ](../../media/File-System-Drop-box-Summary--ACE-image20.png){width="5.0in" height="3.0833333333333335in"}



![](../../media/File-System-Drop-box-Summary--ACE-image21.png){width="5.0in" height="4.493055555555555in"}![](../../media/File-System-Drop-box-Summary--ACE-image22.png){width="5.0in" height="3.5069444444444446in"}



When metadata found new file, it will notice client by WebSocket -- ping , and client will send last version of the file to service






















