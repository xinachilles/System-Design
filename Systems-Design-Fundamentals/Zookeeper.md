# Zookeeper 

Created: 2020-11-28 17:54:12 -0600

Modified: 2020-11-29 01:17:02 -0600

---

Zookeeper is base on the raft protocol



write through primary or leader and read through the follower



each value or context have a zid or version number



what you read is what you write, when you write to system, remeber the zid, when you request a read the zid should greater or equal to that zid



if you need fresh data, you can use sync operation, the replica will process all the write request first
