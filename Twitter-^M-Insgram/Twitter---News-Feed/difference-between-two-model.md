# difference between two model 

Created: 2017-09-16 10:42:00 -0600

Modified: 2017-09-16 10:42:27 -0600

---

![区 别 就 是 有 没 有 News Feed Table. Pull 模 型 是 没 有 News Feed Table 的 ， Pull 只 有 Timeline table. 有 一 些 距 上 的 NewsFeed 和 Timeline 的 定 义 和 我 的 定 义 刚 好 相 反 ， 需 要 注 意 一 下 。 我 的 NewsFeed Table 存 的 是 ， 谁 发 了 什 么 ， 谁 可 以 看 到 。 包 含 三 个 部 分 ， owner_user_id （这 个 newsfeed 是 发 绐 谁 的 ） ， user_id 这 个 内 容 是 谁 发 的 （ 也 可 以 不 存 这 个 ， 因 为 可 以 根 据 tweet 」 d 去 查 是 谁 发 瞓 ， 然 后 tweet 」 d, 然 后 一 些 其 他 的 内 容 。 Timeline Table 存 的 是 ， 谁 发 了 什 么 。 也 是 主 要 包 含 2 个 部 分 ， u 一 id 和 发 的 内 容 。 也 就 是 说 ， 如 果 你 不 做 任 何 优 化 的 话 ， 实 际 上 Timeline Table 是 Tweet Table. 因 为 你 可 以 select ． from tweet table where user d 一 冥 丿 、 〕 京 是 某 丿 、 泊 勺 time 《 ine 了 。 那 么 我 们 看 看 ， 当 你 发 了 一 个 帖 子 之 后 ， 如 果 是 pull 模 型 ， 那 么 只 需 要 在 Tweet Table 里 增 加 一 个 你 发 的 帖 子 的 记 录 就 好 了 。 其 他 什 么 也 不 用 做 ， 当 你 的 好 友 需 要 看 你 的 帖 子 的 时 候 ， 主 动 去 找 你 发 过 的 最 近 100 条 什 么 的 。 而 Push Model 下 ， 你 发 了 一 个 № e 之 后 ， 系 统 需 要 主 动 的 d 刨 ver 你 的 这 个 帖 子 去 到 newsfeed table 里 去 。 比 如 你 有 3 个 好 友 A ， B ， C 。 那 么 系 统 需 要 往 news feed 怡 № 里 存 入 [ A + 你 的 帖 子 l, 但 + 你 的 帖 子 l, [ C + 你 的 帖 刊 三 条 数 据 。 ](../../media/Twitter-^M-Insgram-Twitter---News-Feed-difference-between-two-model-image1.png){width="5.0in" height="2.5555555555555554in"}



