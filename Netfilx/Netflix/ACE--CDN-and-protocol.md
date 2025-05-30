# ACE- CDN and protocol



---

![](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image1.png)



Normal website will let user talked to DNS service and DNS will decide which CND user need to talk to

DNS service and find out the closet CDN

User will talk to the CDN, if the user cannot find the file in CDN then it will talk to the original service and service will save the file to CDN



Next time user will talk to CDN next time







Netflix will store the move to CDN in advance

Like cache ( query cache, if cache has the data, return to user otherwise, go to the DB then update the cache)

Or cache has some data store in cache in advance



![Internet Architecture (ISP, IXP, CDN) ISP A CONTENT PROVIDER B EXCHANGE POINT SWITCH ISP B CONTENT ROVIDER A ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image2.png)

Traffic will first go to ISP











ISP internet service provider ,

Larger peering location for each continent sourth american (100%)



Cdn close to the exchange point iXP



![ISP Network Small Peenng Location Large Peering/Origin Location AWS S3 ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image3.png)



![](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image4.png)



![](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image5.png)

Client will request the video clip, each clip may just 1-2 min, before first clip finish,

It will request next clip from service

















![D 创 用 Li 短 视 视 频 播 放 数 据 流 Allen Nir 应 该 需 ] dawn 《 戗 这 个 具 《 候 笞 出 0 Allen Nin 请 问 这 ， i Type me NetmxinAWS @ 爱 思 版 权 所 有 未 经 允 许 请 勿 录 制 或 传 播 acecodeinterview ℃ om ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image6.png)





CND report health status to AWS

Client request video

AWS check the client permission and check the CDN if has that video

AWS will send the CDN address to user

Client will talk to CDN

CDN may request different resolution of this video from AWS









All the video store in a folder

![Popularon N ~ IVEROALE S P A C E L 0 5 T ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image7.png)



![List of List Service 自 定 义 列 表 （ List) (Trending Now) (Recommend) 流 行 内 容 (Popular) 趋 势 内 容 推 荐 内 容 类 别 内 容 (e.g. sci-fi) ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image8.png)



![Artwork Service ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image9.png)



![Details Service 电 影 剧 集 信 息 分 集 信 息 视 频 播 放 信 息 ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image10.png)



![阿 ル 、 屮 。 叩 ! ハ ー ga O ん 。 ハ ) 佖 ぉ 学 で ・ 叩 い 一 p れ ← と / 51 ル d 0 》 や ! へ *O フ 、 07 叭 ノ 2 よ 、 フ 2 ) ( 互 ) ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image11.png)

Gateway accept the first request and forward to certain service nexttime



![幻 に ロ を を , 爪 ー ↓ CDP し ー ↓ 0 ) ノ ミ 」 者 、 ト ー ( を ・ ′ を d ・ 登 物 し く 第 。 ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image12.png)





![User ID 用 户 数 据 库 UserName Membership ID ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image13.png)



![Membership ID 订 阅 数 据 库 Renewal Start Time Status Offer ID ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image14.png)

Offer id

![内 容 数 据 库 Content ID Parent Description ContentID Content ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image15.png)

Documents store



![Video Video ID 视 频 数 据 库 Thumbnail ContentID ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image16.png)



![Document Store Relational •SWSSS.ZO« MongoDB mt. name: laq_nan•; "J ones % location: I type: "Point", coordinates: 1-73.9876, "Engineer • l, apps: •My App". 1.0.4 2.5.7 j cus: I make: "Bentley", year: ( "Rolls year: J ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image17.png)



![content id: 1, description: "Awesome TV Show", . "tv show" types • seasons: [ season num: 1, description: "Awesome Season One", episodes: [ { episode _ num: 1, description: "Awesome Episode One" , video id: 123, video url: . ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image18.png)

![丨 og ℃ list oflist service 功 能 是 找 到 合 适 的 列 表 和 每 个 列 表 里 的 一 组 Content ID, 具 体 根 据 Content ID 来 找 Thumbnail 的 是 Artwork service. 这 里 有 三 层 的 推 荐 ， 第 一 层 找 到 合 适 的 列 表 ， 第 二 层 找 到 列 表 中 合 适 的 内 容 ， 第 三 层 对 于 每 个 内 容 找 到 合 适 图 片 。 不 重 复 的 。 ](../../media/Netfilx-Netflix-ACE--CDN-and-protocol-image19.png)



















