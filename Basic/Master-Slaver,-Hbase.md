# Master Slaver, Hbase

Created: 2017-04-29 17:40:37 -0600

Modified: 2020-01-31 15:34:01 -0600

---

![Master 一 Slave · 原 理 Write Ah ead Log · SQL 数 据 库 的 任 何 操 作 ， 都 会 以 Log 的 形 式 做 一 份 记 录 · 比 如 数 据 A 在 B 时 刻 从 C 改 到 了 D · Slave 被 激 活 后 ， 告 诉 master 我 在 了 · Master 每 次 有 任 何 操 作 就 通 知 slave 来 读 ℃ g · 因 此 Slave 上 的 数 据 是 有 " 延 迟 " 的 · Master 挂 了 怎 么 办 ？ · 将 一 台 Slave 升 级 (promote) 为 Master, 接 受 读 + 写 · 可 能 会 造 成 一 定 程 度 的 数 据 丢 失 和 不 一 致 master slavel slave2 ](../media/Basic-Master-Slaver,-Hbase-image1.png){width="5.0in" height="2.1597222222222223in"}

from HBase





one of the database we create for the master databsae and other data centers are simply "Slave" database and they replicate the same tables.The Master cluster synchronously sends the logs,the HLogs, over to the Slave dabase,which then use the logsto simply update the corresponding MemStores,which eventually get flushed to disk.





Master - slaver consistency



1.  when update the master database, also ask slaver to read the log and update the slaver database, return successfully only all the database only all database get updating
2.  or remember the write option in the cache, set a expire time, all the read request will check the cache first, if find the value, mean there a write request just happened, read the value from master database



we can have a job to check the data in the master and slaver to make sure those data are same:



there are some ways:

1.  compare the data: compare time is long
2.  compare the log: need a additional action -> write the log
3.  when the write the data, send a message via message bus and a additional service will subscribe the change, we assume the master change and slaver change should happen in 3 second, if we just get master change meassag or just get slaver change message. we need compare the salve and master and update the salver if find different



4.  and compare them : we need implement the message bus and additional service



If the salve service is new, master can sent a snapshot to it.









