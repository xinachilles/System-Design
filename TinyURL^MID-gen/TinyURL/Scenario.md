# Scenario



---

requirement:

long URL to short URL

short URL to long URL and redirect to long URL

custom short URL







**CAP**

1.  The system should be highly available. This is required because if our service is down, all the URL redirections will start failing.
2.  URL redirection should happen in real-time with minimum latency.







![Scenario 需 求 一 一 ． ． 一 QPS + Storage · 1 ． 询 问 面 试 官 微 博 日 活 跃 用 户 · 约 1 開 M · 2 ． 推 算 产 生 一 条 Tiny URL 的 QPS · 假 设 每 个 用 户 平 均 每 天 发 0 ． 1 条 带 URL 的 微 博 · AverageWrite QPS = 100M ． 0 ． 1 / 86400 、 100 · Peak Write QPS = 100 ． 2 = 200 · 3 ． 推 算 点 击 一 条 Tiny URL 的 QPS · 假 设 每 个 用 户 平 均 点 1 个 Tiny URL · Average Read QPS = 100M 鬥 ／ 8 00 、 1k · Peak Read QPS = 2k · 4 ． 推 算 每 天 产 生 的 新 的 URL 所 占 存 储 · 100M ． 0 ， 1 、 10M 条 · 每 一 条 URL 长 度 平 均 1 開 算 ， 一 共 IG · IT 的 硬 盘 可 以 用 3 年 禁 止 录 像 与 传 播 录 像 ， 否 则 将 追 究 法 律 责 任 和 经 济 赔 偿 2k QPS 一 台 SSD 支 持 的 MySQL 完 全 可 以 搞 定 丸 章 法 第 19 页 ](../../media/TinyURL^MID-gen-TinyURL-Scenario-image1.png)



**100 bytes**





**Memory estimates:**If we want to cache some of the hot URLs that are frequently accessed, how much memory would we need to store them? If we follow the 80-20 rule, meaning 20% of URLs generating 80% of traffic, we would like to cache these 20% hot URLs.



Since we have 1K requests per second, we would be getting





200M requests per day.

2K * 86400 = 200 M



To cache 20% of these requests, we would need 4GB of memory.

0.2 * 200M * 100 bytes ~= 4GB

