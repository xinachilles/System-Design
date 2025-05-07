# Facebook onsite

Created: 2020-12-22 16:49:35 -0600

Modified: 2020-12-22 23:34:04 -0600

---

<https://www.1point3acres.com/bbs/thread-656293-1-1.html>





![我 简 单 分 析 一 下 ： 先 从 单 机 入 手 分 析 需 要 些 功 能 ， 我 相 信 大 家 都 能 做 ， 要 注 意 根 据 面 试 官 提 供 的 数 据 具 体 分 析 一 些 情 况 ， 比 如 我 的 面 试 官 说 每 个 网 页 的 大 小 是 100b ， 那 么 说 明 在 一 个 正 常 gbps 的 带 宽 速 度 下 ， 取 网 页 的 速 度 要 快 过 pa ～ ， " t 阉 ct 网 页 的 速 度 ， 那 么 在 山 和 e × tra 之 间 你 就 需 层 保 存 page 然 后 从 单 机 延 展 到 多 机 ， 你 主 要 需 要 解 决 任 务 分 配 的 问 题 ， 我 看 谷 欹 的 做 法 好 像 是 r nd robin, 因 为 网 比 较 经 典 的 面 试 回 答 是 把 网 页 URL 和 机 器 的 id ti r hash norm 誦 ze 在 一 个 有 结 构 的 space 上 ， 比 如 ring, 有 结 构 比 uns r 的 好 处 就 是 较 少 Uoverhead. 接 下 来 的 问 题 是 怎 么 coordinate 这 个 任 务 分 配 ， 你 需 Efip2pKlcentralized coordinationfitradeoffø 选 好 略 以 后 ， 比 如 p2p ， 你 需 要 考 虑 怎 么 存 r ting table, 让 每 个 n e 都 知 道 g viewNiQ 法 scale 的 ， 怎 么 处 理 node 加 入 或 者 离 开 的 情 况 ， 加 入 离 开 时 如何 处 i*nodefistate, 这 个 其 实 就 能 说 不 少 ， 如果你 的 stateæper page 的 那 么 workload reassignment*,t 会 非 常 难 处 理， 选 Emanage state per domain 就 会 好 很 多 ， 还 有 怎 么 处 理 im 《 ance 》 d ， 怎 么 处 埕 f 削 u 「 e (replication), 等 等 。 如 果 还 有 时 间 ， 还可 以 聊 geographic proximity, 有 的 domain " 离 你 的 DC 太 远 了 ， 那 你 就 需 要 m 酣 i 一 up 等 等 希 望 对 你 有 帮 助 0 评 分 ](../../media/Web-crawler-^MP2p-Web-Crawler-Facebook-onsite-image1.png){width="10.083333333333334in" height="1.9479166666666667in"}











<https://www.1point3acres.com/bbs/interview/facebook-software-engineer-641063.html>





<https://www.1point3acres.com/bbs/thread-647088-1-1.html>





<https://www.1point3acres.com/bbs/thread-664451-3-1.html>



<https://www.1point3acres.com/bbs/thread-268942-1-1.html>





