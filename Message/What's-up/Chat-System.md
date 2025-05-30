# Chat System



---

![Applications User Service Friendship Service Message Service Status Service Copyright@ vote".jiuzhang.mm Reciptionist User Service Friendship Service Message Service Status Service ](../../media/Message-What's-up-Chat-System-image1.png)







![Storage • Message Table (NoSQL) • Thread Table (SQL) --- index by Owner (Jeer Id • Thread Id 1 • Participants hash Updated time • NoSQL *fmulti indexes 00:15:13 WhatsAp Audio O Computer audio O Phone call MUTED Soundflower (2Ch) Wilt' • Output Device Talking: Handouts; 1 System Oesian wnatSAog,Od' Questions Audience Question Q: Type question here webinar 140-119-131 G0Tovvebinar Jane Pearson F R'OAY ](../../media/Message-What's-up-Chat-System-image2.png)







![Interviewer: How to Scale? Message NoSQL, Scale Thread user_id i±fi sharding ](../../media/Message-What's-up-Chat-System-image3.png)



改进



![](../../media/Message-What's-up-Chat-System-image4.png)



![Flow • EPA*T-FAppE, Web Server Push Service • AE socket ---push • Éiß Push Server i±4tåiMNl A I. give push server ip 3. ne 2. connect 6. Send message to A Web Server 4 Send message to A 5. Send rhessage to A Push Server ](../../media/Message-What's-up-Chat-System-image5.png)



![Scale 拓 展 · 问 题 群 聊 · 假 如 一 个 群 有 500 人 （ 1m 用 户 也 同 样 道 理 ） · 如 果 不 做 任 何 优 化 ， 需 要 给 这 500 人 一 个 个 发 消 息 · 但 实 际 上 500 人 里 只 有 很 少 的 一 些 人 在 线 （ 比 如 10 人 ） · 但 Message Service 仍 然 会 尝 试 给 他 们 发 消 息 · Message Service (web server) 无 法 知 道 用 户 和 Push Server 的 socket 连 接 是 否 已 经 断 开 · 至 于 Push Server 自 己 才 知 道 消 息 到 了 Push Server 才 发 现 490 个 人 根 本 没 连 上 · Message Service 与 Push Server 之 间 白 浪 费 490 次 消 息 传 递 禁 止 录 像 与 传 播 录 像 ． 否 则 将 追 法 律 责 任 和 经 济 赔 偿 第 19 页 ](../../media/Message-What's-up-Chat-System-image6.png)



![Scale 拓 展 · 解 决 群 聊 章 籌 法 · 增 加 一 个 Channel Service( 频 道 服 务 ） · 为 每 个 聊 天 的 Thread 增 加 一 个 Channel 信 息 · 对 于 较 大 群 ， 在 线 用 户 先 需 要 订 阅 到 对 应 的 Channel 上 · 用 户 上 线 时 ， Web Server (message service) 找 到 用 户 所 属 的 频 道 （ 群 ), 并 通 知 Channel Service 完 成 订 阅 · Channe 僦 知 道 哪 些 频 道 里 有 哪 些 用 户 还 活 着 · 用 户 如 果 断 线 了 ， Push Service 会 知 道 用 户 掉 线 了 ， 通 知 Channel Service 从 所 属 的 频 道 里 移 除 · Message Service 收 到 用 户 发 的 信 息 之 后 · 找 到 对 应 的 channel · 把 发 消 息 的 请 求 发 送 给 Channel Service · 原 来 发 500 条 消 息 变 成 发 1 条 消 息 · Channel Service 找 到 当 前 在 线 的 用 户 · 然 后 发 给 Push Service 把 消 息 Push 出 去 禁 止 录 像 与 传 播 录 像 ． 否 则 将 追 法 律 责 任 和 经 济 赔 偿 第 20 页 ](../../media/Message-What's-up-Chat-System-image7.png)



![Scale Socket Socket Push Service Socket Socket Channel Service Socket Push Service sharding by user id Socket Real-time Service Dispatch messages Subscribe channel Send a message Message Service ](../../media/Message-What's-up-Chat-System-image8.png)



![· 告 诉 服 务 器 我 来 了 / 我 走 了 · 用 户 上 线 之 后 ， 每 隔 3 ． 5 秒 向 服 务 器 hea 戊 beat--- 次 · 服 务 器 告 诉 好 友 我 来 了 / 我 走 了 · 在 线 的 好 友 ， 每 隔 3 ． 5 秒 钟 问 服 务 器 要 一 次 大 家 的 在 线 状 态 · 综 合 上 述 · 每 隔 10 秒 告 诉 服 务 器 我 还 在 ， 并 要 一 下 自 己 好 友 的 在 线 状 态 · 服 务 器 超 过 1 分 钟 没 有 收 到 信 息 ， 就 认 为 已 经 下 线 ](../../media/Message-What's-up-Chat-System-image9.png)



![Channel Service 用 什 么 存 储 数 内 存 就 好 了 ， 数 据 重 要 程 度 不 高 挂 了 就 重 启 因 为 还 可 以 通 过 IOS / Android 的 Push Notification 补 救 ](../../media/Message-What's-up-Chat-System-image10.png)



conversation table

![](../../media/Message-What's-up-Chat-System-image11.png)



![Work Solution 可 行 解 · 用 户 如 何 发 送 消 息 ？ · Client 把 消 息 和 接 受 者 信 息 发 送 给 server · Se 伪 每 个 接 受 者 （ 包 括 发 送 者 自 己 ） 创 建 一 条 Thread （ 如 果 没 有 的 话 ） · 创 建 一 条 message (with thread_id) · 用 户 如 何 接 受 消 息 ？ · 可 以 每 隔 10 秒 钟 问 服 务 器 要 一 下 最 新 的 inbox · 虽 然 听 起 来 很 笨 ， 但 是 也 是 门 先 得 到 这 样 一 个 可 行 解 再 说 · 如 果 有 薪 消 息 就 提 示 用 户 ](../../media/Message-What's-up-Chat-System-image12.png)



![Scale --- Real-time Push Service Socket S Socket Push Service S Socket Socket Push Service Socket S Socket sharding by user id Send a Message Service ](../../media/Message-What's-up-Chat-System-image13.png)



web server 不知道 那个连上了



user subscribe the channel service



pull the message

![](../../media/Message-What's-up-Chat-System-image14.png)![](../../media/Message-What's-up-Chat-System-image15.png)



heart beat, server return a my friends status















