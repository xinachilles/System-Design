# Rate Limiter

Created: 2017-04-30 23:29:20 -0600

Modified: 2017-06-11 17:12:44 -0600

---

![· Scenano 场 ． 鼠 ． 根 据 网 络 请 求 的 特 征 进 行 限 制 （ feature 的 选 取 ） ． IP （ 未 登 录 时 的 行 为 ), User （ 登 录 后 的 行 为 ） ， Email( 注 册 ， 登 录 激 活 ） · 限 制 哪 些 操 作 ？ 一 般 来 说 只 限 制 POST （ 可 以 理 解 为 就 是 会 产 生 写 操 作 的 请 求 ） · Needs 系 统 需 要 做 到 怎 样 的 程 度 · 如 果 在 某 个 时 间 段 内 超 过 了 一 定 数 目 ， 就 拒 绝 该 请 求 ， 返 回 4 错 误 · 2 ／ s ， 引 m ， 10 ， 100 / d ． 无 需 做 到 最 近 秒 ， 最 近 21 分 钟 这 样 限 制 。 粒 度 太 细 意 义 不 大 · Application 应 用 与 服 务 · 本 身 就 是 一 个 最 小 的 Application 了 ， 无 法 再 细 分 · Kilobyte 数 据 存 取 · 需 要 记录@g ） 某 个 特 征 （ feature ） 在 哪 个 时 刻 （ time ） 做 了 什 么 事 情 （ event) · 该 数 据 信 息 最 多 保 留 一 天 （ 对 于 rate=5/m 的 限 制 ， 那 么 一 次 ℃ g 在 一 分 钟 以 后 已 经 没 有 存 在 的 意 义 了 ） · 必 须 是 可 以 高 效 存 取 的 结 构 （ 本 来 就 是 为 了 限 制 对 数 据 库 的 读 写 太 多 ， 所 以 自 己 的 效 率 必 须 高 与 数 据 库 ） · 所 以 使 用 Memcached 作 为 存 储 结 构 〈 数 据 无 需 持 久 化 ） ](../../media/Rate-Limiter-Rate-Limiter-Rate-Limiter-image1.png){width="5.0in" height="2.5104166666666665in"}



![• event+feature+timestamp memcached fikey event=url shorten feature=192. 168.0.1 : memcached.increament(key, ttl=60s) 00:01: 100 00:01: O 00:01: 1 00:02: 10 00:02: 3 • for t in 0---59 do • key = event+feature+(current_timestamp --- t) • sum+= memcahed.get(key, default=O) • Check sum is in limitation ](../../media/Rate-Limiter-Rate-Limiter-Rate-Limiter-image2.png){width="5.0in" height="2.375in"}



![Follow up · 问 ： 对 于 一 天 来 说 ， 有 8 00 秒 ， 检 查 一 次 就 要 86k 的 cache 访 问 ， 如 何 优 化 ？ ， 答 ： 分 级 存 储 之 前 限 制 以 1 分 钟 为 单 位 的 时 候 ， 每 个 № et 的 大 小 是 1 秒 ， 一 次 查 询 最 多 60 次 读 现 在 限 制 以 1 天 为 单 位 的 时 候 ， 每 个 b ket 以 小 时 为 单 位 存 储 ， 一 次 查 询 最 多 24 次 读 同 理 如 果 以 / 寸 为 单 位 ， 那 么 每 个 bu et 设 置 为 1 分 钟 ， 一 次 查 询 最 多 60 次 读 ． 问 上 述 的 方 法 中 存 在 误 差 ， 如 何 解 决 误 差 ？ · 首 先 这 个 误 差 其 实 不 用 解 决 ， 访 问 限 制 器 不 需 要 做 至 哐 色 对 精 确 。 ． 其 次 如 果 真 要 解 决 的 话 ， 可 以 将 每 次 凶 的 信 息 分 别 存 入 3 级 的 bucket （ 秒 ， 分 钟 ， 小 时 ） · 在 获 得 最 近 1 天 的 访 问 次 数 时 ， 比 如 当 前 时 刻 是 23 ： 30 ． 33 ， 加 总 如 下 的 几 项 · 在 秒 的 bucket 里 加 和 23 ． 30 ： 00 · 23 ： ． 33 （ 计 次 查 询 ） · 在 分 的 bucket 里 加 和 23 ℃ 0 · 23 ． 29 （ 计 30 次 查 询 ） · 在 时 的 bu et 里 加 和 佣 一 22 〈 计 23 次 查 询 ） · 在 秒 的 bucket 里 加 和 昨 天 23 ： ： 、 23 ： 30 ： 59 （ 计 26 次 查 询 ） · 在 分 的 bucket 里 加 和 昨 天 23 ： 31 · 23 ： 59 （ 计 29 次 查 询 ） 总 计 耗 费 + 30 + 23 + 26 + 29 = 142 次 询 ， 可 以 接 受 ](../../media/Rate-Limiter-Rate-Limiter-Rate-Limiter-image3.png){width="5.0in" height="3.0625in"}





