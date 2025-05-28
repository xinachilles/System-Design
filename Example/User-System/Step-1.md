# Step 1

Created: 2017-09-24 15:46:06 -0600

Modified: 2017-10-14 10:28:35 -0600

---

![4S ． Scenario, Service, Storage, Scale · Scenario 场 景 · 注 册 、 登 录 、 查 询 、 用 户 信 息 修 改 · 哪 个 需 求 量 最 大 ？ · 支 持 100M DAU · 注 册 ， 登 录 信 息 修 改 QPS 约 · 100M 0 ． 1 / 86400 、 1 開 · 0 ． 1 = 平 均 每 个 用 户 每 天 登 录 + 注 册 + 信 息 修 改 · Peak = 1 佣 · 3 = 300 · 查 询 的 QPS 约 · 100 M ． 1 / 864 開 ～ 100k · 100 = 平 均 每 个 用 户 每 天 与 查 询 用 户 信 息 相 关 的 操 作 次 数 0 看 好 友 ， 发 信 息 ， 更 新 消 息 主 页 ） · Peak = 100k · 3 = 0 k · Service 服 务 一 个 AuthService 负 责 登 录 注 册 一 个 UserService 负 责 用 户 信 息 存 储 与 查 询 一 个 FriendshipService 负 责 好 友 关 系 存 储 禁 止 录 像 与 传 播 录 像 ， 否 则 将 追 究 法 律 责 任 和 经 济 赔 第 4 页 ](../../media/Example-User-System-Step-1-image1.png){width="5.0in" height="2.8472222222222223in"}







![Authentication Service · 用 户 是 如 何 实 现 登 陆 与 保 持 登 陆 的 ？ · 会 话 表 Session · 用 户 Login 以 后 · 创 建 一 个 session 对 象 · 并 把 session_key 作 为 cookie 值 返 回 给 浏 览 器 · 浏 览 器 将 该 值 记 录 在 浏 览 器 的 cookie 中 · 用 户 每 次 向 服 务 器 发 送 的 访 问 ， 都 会 自 动 带 上 该 网 站 所 有 的 cookie · 此 时 服 务 器 检 测 到 cookie 中 的 session 一 key 是 有 效 的 ， 就 认 为 用 户 登 陆 了 丸 章 籌 法 ， 用 户 Logout 之 后 · 从 session table 里 删 除 对 应 数 据 · 问 题 :Session Table 存 在 哪 儿 ？ · A: 数 据 库 session key · B: 缓 存 user id · C ： 都 可 以 expire at 禁 止 录 像 与 传 播 录 像 ， 否 则 将 追 究 法 律 责 任 和 经 济 赔 偿 string Foreign key timestamp Session Table 一 个 hash 值 ， 全 局 唯 一 ， 无 规 律 指 向 UserTable 什 么 时 候 过 期 第 13 页 ](../../media/Example-User-System-Step-1-image2.png){width="5.0in" height="2.7777777777777777in"}



























Step 1 :



what kind of function should be supported.



register, login, edit the user information.



what`s DAU ( daily active user) ? 100M



register, login edit user ( write query)



100 M *0.1 /84600 ~ 100



(0.1 is every day user register, login or edit his information, or write request )



Peek is 100 * 3



read request:( the read request related to the read or search friendship information, such as view friend's time line, get the feed from friends, send message to friend



100 M *100 / 86400 ~ 100 k



100 means every day, user will do 100 operations related to the read query for user information










