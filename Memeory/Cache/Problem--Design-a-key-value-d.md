# Problem: Design a key value data storage support the following



---

Problem: Design a key value data storage support the following



Hashtable with LSMT (lock structure merge tree)

1.Commit log (durability)

2.Memtable

3.SST (cold storage), update, del, compaction

Write => kernel space buffer => hashtable => SST

Read => MOR

MEMTable:

(timestamp_ns, event, location) =>

[(13502312314232313, mock, zoom), (13502312314232313, mock, zoom),.... ] 8G

SST:

(13502312314232313, mock, zoom,tombstone_ns)

...

(13502312314232313, mock, zoom)

Daemon: compact SST









First the data will write into

Commit Log:

R1 -> r2 -> r3 -> r4 (flushed) -> r5

R4 Write to memtable -> In memory data structure:



R2: (12345) -> mocking interview

R4: (12345) -> cooking class







[User case Search]{.mark}





[Limitation on single record]{.mark}: row: key ~ 1 KB value: Y Byte.

~~Array? → 8GB -> single record --- NO~~



[User case 1 search r100, r100 has multiple value]{.mark}

[User case 2 search r501, r501 is not in the database]{.mark}



Hash Table --- Yes

Record : r100 - r501

Top table: Memtable -- hashtable -> O(Log N or Log 1)

| lvl1 SST (disk) r100 - 6

| lvl2 SST (disk) r100 - 4 ST ( suppose r100 has different value in the disk)

| lvl3 SST (disk) r100 - 3



If user is looking for the r100, they will search it in the memory table first

Then search the all level of the sst table and return the latest on base on the time stamp



R501 -> single node → loading from disk to memory -> index -> sst

T3 Lvl 1-> A - Z

T2 Lvl 2 -> A - Z

T1 Lvl 3 -> A - Z

Index: Key - > row#

Bloom filter





R100: lvl 1 --N => read repair → maybe only the latest version



[User case 3 If the key is URL, a large key]{.mark}

{Key: Youtube URL: value: view counts }

8GB = 7Gb is the key key: value = 10/1

Server: 250 GB - 32GB index -> in memory -> 4 SST tree -> page default -> memory 0throttle. → 8GB = < 100MB ( for key)





[User case partition]{.mark}

Composite Key -> ((zipcode: string, user: UUID), activity: UUID, timestamp: Timestamp)





DNS

adhoc01-sf -> 128.12.3.1 X

Adhoc01-sf -> 128.1.23.1

Convert flexible composite key -> key in hashmap

Row-oriented

[

key1(zip,user)

[Activity1, timesatmp1], [Activity2, timesatmp2]...

]

Isolation Level: (MVCC)

3.  Read don't block write, write don't block read

Read and write are different thread and

Read from the original version and writ to snapshot



4.  Single object Transaction

乐观锁

5.  Multiple object transaction in the (one shard or mutiple shard)
6.  Support dump snapshot. Failure cover from snapshot





7.  Optimize on write throughput
8.  Amortized low latency - transactional requirements
9.  Horizontal scalable, if there is new request, we just add new node





Consistent hashing:

Chord: Napster

Record discovery: service / client



[Partition:]{.mark}

1.  Cassandra: proportion to Nodes
2.  Fixed partitions
3.  Dynamic breakdown partition like B tree.

<!-- -->

10. Highly configurable of durability: {Pure memory, persistent}
