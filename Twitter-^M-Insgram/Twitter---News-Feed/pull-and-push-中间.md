# pull and push 中间

Created: 2017-09-21 00:12:37 -0600

Modified: 2017-09-24 17:11:54 -0600

---

![、 LbyAB* 0 1 0 1 年 ， 3 月 前 老 师 当 时 讲 p 十 push 模 型 解 决 明 星 掉 粉 问 题 具 体 是 怎 么 做 的 ？ 有 点 忘 记 了 ， 麻 烦 老 师 冉 说 一 下 具 体 思 路 。 非 常 感 谢 1 个 回 复 东 邪 黄 药 用 设 立 一 个 中 间 的 缓 冲 地 帚 ： 1. 明 星 ： > = 1m 粉 丝 的 2 伪 明 星 ： > = 10k ， < 1m 粉 丝 的 3 ． 普 通 人 ： < 10k 的 当 这 些 用 户 发 帖 子 时 ： 1 ． 明 星 ： 不 push 2 伪 明 星 ： push 绐 所 有 粉 丝 3 ． 普 通 人 ： push 给 所 有 粉 丝 当 这 些 用 户 看 朋 友 圈 时 ， 所 有 人 都 做 如 下 步 骤 ： 1 ． 主 动 得 到 自 己 关 注 的 明 星 和 伪 明 星 的 最 新 的 帖 子 2 把 这 些 帖 子 ， 和 我 关 注 的 人 push 绐 我 的 帖 子 ， 合 并 在 一 起 显 示 2017 ． 02 ． 26 这 样 做 的 好 处 是 ， 一 个 明 星 控 粉 的 时 候 ， 不 会 一 下 子 从 明 星 掉 成 普 通 人 。 会 有 一 个 掉 成 伪 明 星 的 过 程 。 这 样 ， 即 便 发 帖 的 时 候 ， 他 是 明 星 ， 粉 丝 看 帖 的 时 候 ， 他 掉 为 了 伪 鎸 星 ， 但 也 不 影 粉 丝 仍 然 可 以 通 过 主 动 的 p 得 到 他 发 的 帖 子 。 ](../../media/Twitter-^M-Insgram-Twitter---News-Feed-pull-and-push-中间-image1.png){width="5.0in" height="4.173611111111111in"}









![Scale 扩 展 一 Lady Gaga ． Push 结 合 Pull 的 优 化 方 案 · 普 通 的 用 户 仍 然 Push · 将 Lady Gaga 这 类 的 用 户 ， 标 记 为 明 星 用 户 · 对 于 明 星 用 户 ， 不 Push 到 用 户 的 News Feed 中 · 当 用 户 需 要 的 时 候 ， 来 明 星 用 户 的 Timeline 里 取 ， 并 合 并 到 News Feed 里 ． 摇 摆 问 题 · 明 星 定 义 · followers > 1m · 邓 超 掉 粉 · 邓 超 某 天 不 停 的 发 帖 刷 屏 ， 于 是 大 家 果 取 关 ， 一 天 掉 了 几 十 万 粉 · 解 决 方 法 ． 明 星 用 户 发 Tweet 之 后 ， 依 然 继 续 Push 他 们 的 Tweet 到 所 有 用 户 的 News Feed 里 · 原 来 的 代 码 完 全 不 用 改 了 · 将 关 注 对 象 中 的 明 星 用 户 的 Timeline 与 自 己 的 News Feed 进 行 合 并 后 展 示 · 但 并 不 存 储 进 自 己 的 News Feed 列 表 ， 因 为 Push 会 来 负 责 这 个 事 情 。 ](../../media/Twitter-^M-Insgram-Twitter---News-Feed-pull-and-push-中间-image2.png){width="5.0in" height="3.0625in"}




