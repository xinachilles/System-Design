# What's up - no history message -ACE

Created: 2021-03-25 22:17:44 -0600

Modified: 2021-05-27 15:33:36 -0600

---

**[Functional Requirements:]{.mark}**

1.  Messenger should support one-on-one conversations between users.

<!-- -->

2.  Messenger should keep track of online/offline statuses of its users.
3.  Support message status (read unread..)

[Non function requirement]{.mark}



Support billion of user

High Availability

low latency Users should have real-time chat experience with minimum latency.

Consistency -- don't lose message, message order



![功 能 性 非 功 能 性 需 求 1.2 理 解 需 求 用 户 通 过 手 机 网 络 交 流 （ 一 对 一 聊 天 ， 消 息 状 态 ， 用 户 在 线 状 态 ） 用 户 发 送 的 信 息 一 旦 被 送 达 另 一 个 用 户 ， 信 息 必 须 从 服 务 器 上 完 全 删 除 高 扩 展 性 ， 海 量 用 户 高 可 用 性 ， 服 务 不 宕 机 极 低 延 迟 ， 消 息 第 一 时 间 送 达 一 致 性 ， 消 息 不 丢 失 ， 不 乱 序 隐 私 ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image1.png){width="5.0in" height="2.625in"}





![Sent 服 务 器 接 收 到 信 息 消 自 状 态 Deliver 消 息 到 达 接 收 方 App Seen 消 息 被 接 收 方 看 到 ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image2.png){width="5.0in" height="2.8541666666666665in"}





Low latency --- push

Protocol - websocket





![Server Websocket HTT? handshôke webSockeES FVS iSEer,E close webSockeES clieb•lb ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image3.png){width="5.0in" height="4.451388888888889in"}







![思 考 题 Websocket 单 机 最 多 能 维 持 多 少 Connection? 可 能 达 到 IOM 〗 许 请 勿 录 制 或 传 播 acecodei nterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image4.png){width="5.0in" height="4.520833333333333in"}





1 service can have multiple IP address 2^64





![CACIIt 假 设 瓶 颈 服 务 资 源 存 储 资 源 1 · 3 资 源 估 算 1B DAU ， 50%maintainsconnection 100Message Sent/Receivedperday #0fOngoingSessions #0fOngoingSessions-1B*50%= 5 （ M MessageDelivery Rate-IOO*IB ／ （ 24 36 闐 ） 、 =1.2M In-flightChatStoæ (assumeldayofmessage) 1 严 1B 100Byte (msgsize) ： 100TB 0 爱 思 版 权 所 有 未 经 允 许 请 勿 录 制 或 传 播 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image5.png){width="5.0in" height="3.1527777777777777in"}





Data flow



Sender online/ offline -- receiver online/ offline



1.  Sender online, receiver online -- send message



![ه Messaøinq ئ (تلمصانرا) حححا لسماع t-tm&ne«sutder&Reup12 %.*&.cs(oc.ecodet;t±eniu.o ل ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image6.png){width="5.0in" height="2.8680555555555554in"}



2.  Sender online receiver offline -- send message

B Pull message

![二 ← ロ , ー 圍 2 亘 ← = て 區 ロ ー 、 ~ 乙 ( 1 つ つ つ Ⅵ 95 ! コ 0 ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image7.png){width="5.0in" height="2.736111111111111in"}







Update status: A and B both online

![-nnxčS 7Vrpo 8 Snpą.S ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image8.png){width="5.0in" height="3.125in"}![Q9ACE Write-Back Write to cache Application Write to Cache Database acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image9.png){width="5.0in" height="3.2847222222222223in"}





![^ 0 を 十 材 ぞ 55 久 当 。 、 し f の レ 。 B seas 、 セ 山 い ト 'Q 仙 ン ゞ れ 5 セ 社 ー 0 愛 思 版 扠 所 有 未 経 允 i 午 、 勿 泉 制 或 福 " odei 徹 0 Ⅳ iew て om ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image10.png){width="5.0in" height="2.798611111111111in"}



After B seen the message, it will write a new message 1- >2 ->3 and store in DB



After A go online, it will read the message status from DB













![思 考 题 消 息 状 态 怎 么 存 储 ？ 把 消 息 状 态 当 作 一 条 新 的 消 息 0 爱 思 版 权 所 有 未 经 允 许 请 勿 录 制 或 传 播 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image11.png){width="5.0in" height="3.0208333333333335in"}

Queue can be in service or out of service

![@"e（虱air愫"瞌易 ， 1 0 爰 思 版 权 所 有 未 经 允 许 诺 勿 录 制 或 传 播 acecodeinterview ℃ om ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image12.png){width="5.0in" height="2.8333333333333335in"}![思 考 题 保 证 隐 私 的 前 提 下 ， 如 何 容 灾 ？ 以 客 户 端 为 Source of Truth 0 爱 思 版 权 所 有 未 经 允 许 请 勿 录 制 或 传 播 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image13.png){width="5.0in" height="2.8125in"}

Client side can re-try

![](../../media/Message-What's-up-What's-up---no-history-message--ACE-image14.png){width="5.0in" height="3.125in"}

![思 考 题 Sent 状 态 何 时 更 新 ？ webs 。 cket 发 送 信 息 到 服 务 器 的 时 候 ， 服 务 器 接 收 既 可 给 客 户 端 ACK. 0 爰 思 版 权 所 耒 经 允 许 请 勿 录 制 或 传 播 acecodeintervievv.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image15.png){width="5.0in" height="3.0694444444444446in"}![思 考 题 liver, / S n 状 态 何 时 更 新 ？ Deliver/Seen 消 息 需 要 由 收 件 人 发 出 诓 回 到 发 件 人 @ 爱 思 版 权 所 有 未 经 允 许 请 勿 录 制 或 传 播 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image16.png){width="5.0in" height="3.0833333333333335in"}![思 考 题 如 何 保 证 消 息 顺 序 ？ Queue 客 户 端 排 序 0 爰 思 版 权 所 有 耒 经 允 许 勿 录 刮 传 播 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image17.png){width="5.0in" height="3.0208333333333335in"}![思 考 题 队 列 怎 么 存 消 息 ？ 以 收 件 人 还 是 发 件 人 Sha ？ 收 件 人 0 爱 思 版 所 有 禾 经 允 许 请 勿 录 制 或 传 播 acecodeinterview ℃ 0甬 囗 囗 囗 匚 囗 囗 囗 匚 囗 囗 囗 匚 ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image18.png){width="5.0in" height="3.0694444444444446in"}![0 00《@0《00 思 考 题 客 户 端 怎 么 把 消 息 状 态 跟 对 应 的 消 息 匹 配 ？ Hash or Message ID 0 爱 思 版 权 所 有 耒 经 允 许 请 勿 录 制 或 传 播 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image14.png){width="5.0in" height="3.125in"}

Notification service if user is offline

![APNS Apple Google MQtrr IOT pub/sub Open Standard @ acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image19.png){width="5.0in" height="3.048611111111111in"}



![(M 巧 鬥 《 & ～ 国 P 牖 凵 。 。 以 0 。 5 驴 L-e 韩 扣 以 se ' 丷 泛 之 S 以 ． 疒 0 爱 思 版 椒 所 有 耒 经 允 许 请 勿 录 制 或 传 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image20.png){width="5.0in" height="3.0in"}

API -- WebSocket -RPC

![SEND destination : /messages " receipient_id" : 123, "message "62kd890" , 1610435816 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image21.png){width="5.0in" height="3.0694444444444446in"}



Unread message

![ej ACE Message ID Staging Message Table Primary Key -> (Recipient ID, Ilmestamp, Sender ID) Encrypted Recipient ID Timestamp Sender ID Partition/Shard Key Clustering Key @ acecodeinterviewcom' ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image22.png){width="5.0in" height="3.173611111111111in"}

![BOB send Me S Bob uses Alice's public key to encrypt Servers Encrypted e-mail to 6ekd8900ptak1 ALICE 6ekd8900ptak1 Alice uses her private key to decrypt the Alice' public key Bob •s PLHic key ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image23.png){width="5.0in" height="3.1527777777777777in"}

Bob has alice's public key and use alice public key to send message to alice





Clustering key = order key



![1 · 8 扩 展 性 服 务 器 StatelessService 存 储 分 布 式 缓 存 ， 分 布 式 数 据 库 的 爱 思 版 椒 所 有 耒 经 允 许 请 勿 录 制 或 传 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image24.png){width="5.0in" height="2.875in"}



![服 务 器 存 储 1 · 9 容 灾 设 计 ur ofTru 山 在 客 户 端 Replication ()B ， 山 e ） 0 爱 思 版 椒 所 有 耒 经 允 许 请 勿 录 制 或 传 思 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image25.png){width="5.0in" height="3.0277777777777777in"}



![Ongoing 1 · 10 监 控 警 报 9 MessageDelivery 9 MessageQueue Health @ 爱 思 版 椒 所 有 耒 经 允 许 请 勿 录 制 或 传 acecodeinterview.com ](../../media/Message-What's-up-What's-up---no-history-message--ACE-image26.png){width="5.0in" height="2.8125in"}


























