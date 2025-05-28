# Cassandra 存 friendship

Created: 2017-09-24 18:04:33 -0600

Modified: 2017-09-24 18:08:35 -0600

---

![Cassandra Friendship Table , row_key user idl ' column _ key <friend user id2> ' value <is mutual friend, is_blocked, timestamp> is <friend user id3> <is mutual friend, blocked, timestamp> is user id2 <friend user idl> <is mutual friend, blocked, timestamp> user id3 <friend user idl> <is mutual friend, is_blocked, timestamp> ](../../media/Example-User-System-Cassandra-存-friendship-image1.png){width="5.0in" height="2.5347222222222223in"}![Cassandra 存 friendship 的 例 子 、 L by n 同 学 2 0 8 月 前 Ta ： 系 统 设 计 、 L Ta: Jing Guo ， 怎 么 查 找 某 人 关 注 的 人 东 邪 老 师 课 上 讲 的 例 子 ， row_key(user_id), col_key(friend_user_id), 怎 么 找 某 人 所 有 关 注 的 人 呢 ？ 找 粉 丝 的 话 ， 这 样 可 以 吗 ： query(id, 0 ， INT32_MAX)? 2 个 回 复 九 章 管 理 员 2017 ． 02 ． 13 可 以 啊 ， 就 是 这 么 做 。 Cassandra 中 ， column_start 和 column_end 是 可 以 不 指 定 的 ， 不 指 定 代 表 找 到 投 。 所 以 是 query(id, null, null) ](../../media/Example-User-System-Cassandra-存-friendship-image2.png){width="5.0in" height="2.6180555555555554in"}![X bys* • 1 C) 11 B, 1 Bfij row_key: user_id column_key: timestamp+friend_user_id; 1. query? 3. R-ækeep sorted so we can utilize the binary search? ](../../media/Example-User-System-Cassandra-存-friendship-image3.png){width="5.0in" height="1.8125in"}![](../../media/Example-User-System-Cassandra-存-friendship-image4.png){width="5.0in" height="3.0416666666666665in"}




