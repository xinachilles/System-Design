# Zookeeper 



---

Zookeeper is base on the raft protocol



write through primary or leader and read through the follower



each value or context have a zid or version number



what you read is what you write, when you write to system, remeber the zid, when you request a read the zid should greater or equal to that zid



if you need fresh data, you can use sync operation, the replica will process all the write request first
