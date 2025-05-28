# google calendar

Created: 2017-05-04 20:59:19 -0600

Modified: 2017-05-04 21:00:12 -0600

---

![Scenano 1. createevent 2 ． invite user 3 ． notiß/ users at specific time 4 ． notw user periodically Service 1 ． User Service. 负 责 管 理 用 户 数 据 。 2 Event Semce. 负 责 管 理 户 创 建 的 Events 3 ． Notification Service- 负 责 n ， 用 户 某 个 event 到 了 Storage 这 个 题 考 点 并 不 在 存 在 什 么 数 据 库 里 ， 所 以 应 该 不 会 追 问 。 主 要 的 考 点 会 是 问 Event 要 怎 么 存 来 实 现 notify user at specific time 和 periodically. Event 大 概 需 要 存 created 可 ， { 谁 创 建 的 ） strategy { 一 次 性 的 ， 还 是 周 期 的 。 如 果 是 周 期 的 ， 那 么 周 期 的 策 略 是 什 么 ， 每 周 ， 还 是 每 周 耒 ， 还 是 什 么 ， 可 以 用 一 个 具 体 的 结 构 化 数 据 表 示 出 来 ， 并 艹 " 巨 ze 成 1 艹 n 之 类 的 存 起 来 ） next notify_time （ 下 一 次 什 么 时 候 提 醒 ， 注 意 这 里 是 下 一 次 什 么 时 候 提 醒 ， 而 不 是 第 一 次 什 么 时 候 提 醒 ） attendes: many 乜 0 many 自 勺 s （ 实 际 上 会 存 成 另 夕 卜 一 张 表 ， 记 录 哪 个 用 户 爹 与 了 哪 个 event) ](../../media/Example-google-calendar-google-calendar-image1.png){width="5.0in" height="3.78125in"}



![下 面 整 理 一 个 完 整 的 流 程 ， 1 ． create event 就 是 数 据 庫 的 create 呗 。 2 invite user- 就 是 在 manytomany 的 表 里 加 一 条 记 录 ， 顺 便 记 录 一 个 状 态 ， 这 个 用 户 接 受 了 邀 请 没 有 。 3 ． notiß/ user at specific time, 首 先 我 们 需 要 搞 清 楚 ， 到 底 要 notify 什 么 。 这 里 有 这 么 几 种 可 能 性 ： A. 手 机 上 弹 出 一 个 notification 提 醒 用 户 。 这 个 非 常 简 单 ， 这 个 不 用 走 服 务 器 那 边 。 只 需 要 用 户 的 手 机 每 隔 一 段 时 间 pull 一 下 最 新 数 据 ， 然 后 将 需 要 提 醒 的 event ， 按 照 next_notify_time 倒 序 ， 然 后 用 一 个 r 豳 r 的 进 程 每 分 钟 看 一 眼 就 行 了 。 因 为 一 个 用 户 登 陆 了 自 己 的 账 户 之 后 ， 才 会 被 提 醒 。 B. 蛤 用 户 发 一 封 邮 件 之 类 的 进 行 提 醒 。 这 个 就 比 较 唯 了 ， 因 为 需 要 通 过 服 务 器 去 纷 发 。 做 法 可 以 是 这 样 ， 首 先 我 们 需 要 一 个 MessageQueue 来 做 提 醒 任 务 的 Broker （ 也 是 消 息 队 列 来 存 需 要 马 上 去 提 醒 的 任 务 ） ， 然 后 有 一 个 进 程 不 断 扫 描 数 据 库 里 的 已 经 到 了 需 要 提 醒 的 时 间 点 （ 保 睑 起 见 ， 减 去 1 分 钟 ） ， 但 是 还 没 有 提 醒 的 Events- 然 后 把 这 些 events 标 记 为 正 在 提 醒 ， 丢 给 MessageQueue, 然 后 启 动 一 些 专 门 复 杂 发 邮 件 的 机 器 ， 订 阅 MessageQueue, 这 些 机 器 得 到 了 任 务 之 后 ， 就 去 发 邮 件 ， 并 且 把 任 务 标 记 为 已 经 完 成 。 l. n ， user periodically 其 实 看 起 来 有 点 难 ， 实 际 不 难 ， 只 要 你 在 第 3 步 中 完 成 了 提 醒 之 后 ， $Gstrategyü--- 下 这 个 evente 现 在 就结 束 呢 ， 还 是 还 有 下 一 次 提 醒 。 如 果 还 有 下 一 次 提 醒 ， 计 算 好 下 一 次 的 时 间 ， 然 后 存 更 新 一 下 event 的 next_notify_time7 好 了 。 ](../../media/Example-google-calendar-google-calendar-image2.png){width="5.0in" height="3.2604166666666665in"}



![Scale Sca 要 看 面 试 官 问 你 什 么 具 体 的 问 题 你 具 体 去 答 了 。 从 我 看 来 ， 如 果 是 手 机 那 边 的 提 醒 ， 其 实 没 什 么 的 ， 因 为 是 手 机 ca 《 自 已 提 醒 ， 所 以 只 要 定 期 Pull 新 数 据 就 好 了 。 这 个 很 容 易 scale- 主 要 唯 Sca 的 是 ， 如 果 是 发 邮 件 提 醒 的 那 种 events ， 要 从 服 务 器 端 来 处 理 。 那 么 一 个 进 程 扫 描 整 个 数 据 庫 看 看 到 时 间 的 events 有 哪 些 这 个 是 很 慢 很 慢 的 。 所 以 需 要 加 机 器 ， 通 过 s № 忆 ing 分 开 处 理 。 我 的 想 氵 去 是 按 照 的 user_id 进 行 sharding 来 让 events 存 在 不 同 的 数 据 库 里 。 然 后 弄 若 干 个 进 程 ， 每 个 进 程 负 责 一 个 m00 sharding （ 一 个 进 程 负 责 不 了 一 整 个 数 据 库 ， 所 以 可 以 按 照 m00 sharding 来 分 任 务 ） ， 然 后 每 个 进 裎 Regular 的 扫 描 自 己 的 m00 sharding 中 到 期 的 events, 然 后 创 建 提 醒 任 务 ， 丢 给 MessageQueue- 然 后 你 可 能 会 问 MessageQueue 会 不 会 是 瓶 颈 ， 答 案 是 一 般 不 会 ， MessageQueue 的 性 能 是 很 强 的 ， 而 且 很 MessageQueue 的 技 术 很 成 熟 ， 可 以 有 双 master 之 类 的 模 式 来 保 证 自 己 不 挂 。 另 外 就 是 MessageQueue 也 可 以 有 很 个 呀 ， 你 指 定 好 一 个 规 则 让 不 同 的 扫 描 进 程 发 送 给 不 同 的 Message Queue 好 了 。 ](../../media/Example-google-calendar-google-calendar-image3.png){width="5.0in" height="2.1666666666666665in"}





