# Thread table

Created: 2017-10-06 15:18:06 -0600

Modified: 2017-10-06 15:24:14 -0600

---

![](../../media/Message-What's-up-Thread-table-image1.png){width="7.708333333333333in" height="13.083333333333334in"}



![16 期 第 9 课 第 8 页 THREAD TABLE" 蟲 TA ： 濮 助 教 0 1 个 回 复 0 j 同 学 7 个 月 16 天 1 9 小 时 36 分 钟 前 thread table 中 有 participant_cash 说 是 avoid duplicate threads, 但 thread id 定 义 为 create userid+timestamp, 怎 么 会 有 重 复 丿 为 何 需 要 participant_hash 作 为 一 部 分 key ？ 东 邪 黄 药 师 7 个 月 1 6 天 26 分 钟 前 很 高 兴 你 理 解 了 participant_hash 用 来 判 断 重 复 的 。 这 里 的 重 复 ， 有 两 个 理 解 方 式 ： 1 ． 数 据 库 角 度 的 数 据 重 复 。 比 如 你 说 的 thread_id ： create_user id + timestampo 那 么 根 据 这 个 d 的 构 造 可 以 看 出 ， 是 不 会 引 起 数 据 重 复 的 。 2 ． 产 品 角 度 的 重 复 。 你 使 用 过 微 信 应 该 会 有 个 经 验 （ 我 课 上 也 反 复 提 到 了 ） ， 你 可 以 多 次 创 建 属 于 A B c 三 个 人 的 群 聊 。 比 如 A 创 建 了 一 个 三 个 人 的 群 聊 ， participant_hash ： "ABC" ， create user id ： A, thread id ： "A+timestamp"o 但 是 这 个 时 候 B 想 要 和 A 和 c 聊 天 ， 那 么 他 也 创 建 了 一 个 thread ， 然 后 thread id 就 等 于 "B+timestamp" ， 这 个 时 候 ， thread id 并 不 冲 突 ， 但 是 participant_hash 却 重 复 了 ， 这 样 就 能 够 避 免 ABC 三 个 人 有 多 个 不 同 的 群 聊 。 你 可 能 会 问 ， 那 么 把 participants 的 信 息 都 放 到 thread ℃ 里 可 以 不 呢 ？ 答 案 是 不 行 ， 因 为 participants 是 一 个 变 化 的 数 据 ， 你 可 以 随 时 加 新 的 人 到 群 聊 当 中 。 不 像 create c 」 ser id 和 create_timestamp 是 一 《 不 变 的 量 。 ](../../media/Message-What's-up-Thread-table-image2.png){width="9.177083333333334in" height="13.083333333333334in"}




