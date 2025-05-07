# Hot Spot

Created: 2017-09-16 15:43:23 -0600

Modified: 2017-09-16 17:39:36 -0600

---

![](../../media/Twitter-^M-Insgram-Twitter---News-Feed-Hot-Spot-image1.png){width="10.083333333333334in" height="4.958333333333333in"}



![](../../media/Twitter-^M-Insgram-Twitter---News-Feed-Hot-Spot-image2.png){width="10.083333333333334in" height="5.5625in"}







![设 计 twitter 题 里 的 TweetListTable 在 disk 中 储 存 的 问 题 、 L by J 同 学 3 0 1 年 ， 6 月 前 在 第 六 堂 课 中 的 设 计 bw 献 er 题 中 》 我 们 用 TweetLis 仃 äb 来 槠 存 娜 个 用 户 发 了 娜 条 № eet. 它 在 disk 中 的 储 存 方 式 ， 是 以 TweetlD 为 key, 进 而 决 定 某 个 № e 酬 D 和 us 丽 D 的 key value pair 要 存 放 在 一 台 " er 的 哪 一 个 chunk. 对 此 我 有 以 下 疑 问 ： TweetlD 會 随 著 户 发 出 这 条 tweet 的 时 间 而 涕 增 ， 那 麽 最 近 发 出 的 那 些 № e 是 否 都 会 被 存 到 同 一 台 " er 的 同 一 个 chunk 呢 ？ 这 样 的 话 会 不 会 有 严 重 的 hot spot 问 题 呢 ？ ](../../media/Twitter-^M-Insgram-Twitter---News-Feed-Hot-Spot-image3.png){width="10.083333333333334in" height="3.3125in"}



![九 章 管 理 员 hot spc&%cache 解 决 ， 不 能 靠 DB 解 决 。 九 章 管 理 员 严 肅 的 回 答 一 下 ： 你 的 这 个 问 题 是 个 好 问 题 。 答 案 是 不 会 。 首 先 取 决 于 你 的 台 机 器 的 db 之 间 的 关 系 是 什 么 。 2016 ． 03 ． 01 2016 ． 03 ． 01 如 果 是 SQL 的 数 据 库 ， 按 D 进 行 sha 忆 ， 一 般 的 做 法 就 是 ％ 总 的 机 器 数 。 所 以 在 若 干 机 器 上 分 布 得 是 比 较 均 匀 的 。 如 果 是 NoSQL 的 数 据 库 ， consistent hashing, 那 就 更 是 随 机 分 布 。 你 所 说 的 hot spo 说 ， 因 为 近 的 帖 子 id 都 差 不 多 的 ， 所 以 大 瘃 读 的 比 较 多 。 这 个 是 不 会 出 现 问 题 的 。 在 一 个 chunk 里 是 好 事 丿 匕 硬 盘 读 取 数 据 更 快 了 ， 一 囗 气 全 拿 出 来 。 真 正 要 考 虑 的 hot spot 问 题 是 这 样 的 ： 某 个 大 明 星 ， 比 如 lady gaga, 发 了 个 帖 子 ， 这 个 帖 子 很 多 人 访 问 ， 那 么 请 求 某 个 固 定 冠 的 tweet 的 q " 变 多 。 这 个 才 会 产 生 hot spot- 这 个 的 解 决 办 法 是 駡 cache 。 主 要 需 要 讨 论 是 的 cac № 失 效 之 后 ， 如 果 que 时 间 比 较 长 （ 回 填 时 间 很 长 ） 怎 么 办 ， 数 据 库 无 法 承 受 同 时 一 大 堆 来 我 的 《 ong time query, 会 挂 扌 軋 解 决 办 法 是 fa b k 扩 展 了 mem che ， 加 了 一 个 叫 做 《 ea 一 get 的 东 西 ， 会 自 动 检 测 这 个 key 是 不 是 一 个 hot key, 如 果 是 的 话 ， 标 记 一 下 现 在 是 不 是 正 在 取 数 据 库 请 求 数 据 的 路 上 ， 如 果 是 的 话 ， 就 让 memcachefiiä*hold 住 ， 而 不 是 返 回 没 有 。 一 般 我 们 的 逻 辑 是 men ache 没 有 的 话 ， 去 DB 请 求 ， 这 样 就 会 造 成 在 che 失 效 的 时 候 ， DB 请 求 突 然 增 。 厄 ase 一 t 和 《 ease 一 t 解 决 了 这 个 问 题 。 你 可 以 搜 一 下 facebook 的 memcache 的 下 。 ](../../media/Twitter-^M-Insgram-Twitter---News-Feed-Hot-Spot-image4.png){width="10.083333333333334in" height="6.864583333333333in"}






