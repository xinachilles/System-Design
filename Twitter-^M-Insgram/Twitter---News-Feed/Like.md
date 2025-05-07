# Like 

Created: 2017-09-16 15:23:24 -0600

Modified: 2017-10-10 00:44:09 -0600

---

![Scale --- ACIfiJF-fi% Likes? Tweet Table id user id content created at like nums * comment nums * retweet nums * integer Foreign Key text timestamp integer integer integer id user id tweet id created at Like Table * integer Foreign Key Foreign Key timestamp De-normalize ](../../media/Twitter-^M-Insgram-Twitter---News-Feed-Like-image1.png){width="10.083333333333334in" height="6.114583333333333in"}



![蟲 TA ： 0 关 于 DESIGN TWITTER 里 的 LADY GAGA 发 推 之 后 的 HOT SPOT" A 同 学 1 年 4 个 月 1 5 天 13 小 时 45 分 钟 前 老 师 能 解 答 一 下 design twtter 中 的 hot spot 问 题 么 ， PPT 说 会 在 微 信 公 众 号 解 答 ， 但 没 有 找 到 ， 主 要 是 下 面 几 个 问 题 1 ． 如 果 用 c 瓿 he 来 解 决 hot spot 问 题 ， 那 么 like ， retweet ， comment 这 几 个 操 作 都 会 修 改 tweet 的 基 本 信 息 ， 如 何 更 新 ？ 2 cache 失 效 如 何 破 ？ ](../../media/Twitter-^M-Insgram-Twitter---News-Feed-Like-image2.png){width="10.083333333333334in" height="4.34375in"}



![0 1 0 东 邪 黄 药 师 1 年 1 个 月 8 天 1 小 时 12 分 钟 前 用 cache through 不 用 cache aside 可 以 解 决 这 个 问 题 。 j 同 学 1 年 1 个 月 3 天 14 小 时 57 分 钟 前 hotspot 是 一 个 很 好 应 用 Read Thr ℃ ugh / write thr ℃ ugh 的 例 子 ． 说 人 话 ： Distribl 」 ted Cache system 一 般 有 2 种 design partten ： 1 Cache aside Cache aside 要 求 Application 来 管 理 c 瓿 爬 内 容 ， 保 证 数 据 一 致 性 ； 当 读 取 时 ， 直 接 读 取 C 瓿 爬 ； 如 果 没 有 数 据 或 数 据 inv 訕 d ， 那 么 就 要 读 tasto 鼢 ， 读 完 以 后 再 用 datastore 的 值 写 入 山 e 。 当 写 入 时 ， 先 写 Data store ， 再 invalid c 瓿 he ． 〔 下 一 次 读 就 强 制 先 读 Datast 引 创 。 2 Read Through/Write Thr ℃ ugh Application 层 就 把 cache 当 datastore ， cache 的 c 」 pdate 由 cache 自 己 保 证 ； c 瓿 爬 会 通 过 和 ta store 的 异 步 操 作 来 礼 to refresh 自 己 ， 这 样 ca 山 e 自 动 就 是 最 新 的 ， 而 且 由 于 是 异 步 操 作 ， 避 免 了 peak time q 虻 ry 过 多 的 问 题 。 当 Caddy g 3 发 新 推 文 的 时 候 ， 很 多 用 户 like ， comm " t ， 造 成 同 一 条 推 文 反 复 修 改 ； 当 采 用 WT 模 式 的 时 候 ， 推 文 直 接 在 c 瓿 爬 里 面 修 改 ， 立 即 返 回 用 户 ， 然 后 c 爬 在 DB 不 忙 的 时 候 再 更 新 D 日 ； 这 样 就 降 低 了 latancy ， 并 且 减 少 了 D 日 峰 值 压 力 。 东 邪 黄 药 师 1 年 1 个 月 3 天 12 小 时 6 分 钟 前 点 赞 j 同 ](../../media/Twitter-^M-Insgram-Twitter---News-Feed-Like-image3.png){width="9.666666666666666in" height="13.083333333333334in"}





