# Cassandra



---

1.  given a key value pair, how do you know which key was stored in which server



Cassandra uses the ring . Cassandra places servers in a virtual ring; again the ring here consists of two power m points and here is 7, so it's 128 points.



the key gets stored at servers that are successors to its point on the ring. So, for instance key with, with an ID of 13, which maps to 13 will get stored at servers 16 and also 32, and also 45.



One of these might be the primary replica, one of these the others might be backup replicas. As far as Cassandra is concerned it doesn't necessarily need to have primary or backup replicas,



The client sends its queries to the coordinator. when the client sends a query to the coordinator, the coordinator simply forwards the query to the appropriate replicas or just some of the replicas in for that particular key. This means that every server, which could be the coordinator, needs to know about where the keys are stored on every other server in the system.





![Say m=7 N112 (Remember this?) N96 Read/write K 13 Client O N80 Coordinator One nng per DC N16 Primary replica for key K13 N32 N45 Backup replicas for key K13 Cassandra uses a Ring-based DHT but without finger tables or routing Key *server mapping is the "Partitioner" ](../media/Cassandra-Cassandra-image1.png)







Partition



So there are different kinds of replication strategies there are two main classes that are supported by Cassandra, they are the SimpleStrategy and the Network Topology Strategy.



The SimpleStrategy uses the partitioner and there are two kinds of partitioners.



The random partitioner uses hash-based partitioning just like Chord : keys are hashed to the point in the ring and then are stored at servers that are closed to that point in the ring, or assigned to that, are assigned to that segment in the ring itself.





There's also byte order partitioner which assigns ranges of keys to servers. You might want to maintain a, a table in Cassandra that preserves the ordering of the keys in there. For instance, the keys might be timestamped, and you might want to maintain the ordering of the timestamps.



This is useful for range queries. For instance if you want to get all the Twitter users starting with a or b then you can do a range query very easily if you're using a byte order partitioner, by simply searching for the servers that will be storing that particular range of keys. If you were using the hash based partitioning instead, you'd essentially have to ask every single server in that particular ring, to see if the server has any key value key value pairs that match this particular range.





The Network Topology Strategy





handle failure



So, keys always need to be writable, so you want to make sure that even if you have failures, the writes succeed and return an acknowledgement to the client. So for this, Cassandra uses what is known as a hinted handoff mechanism.



This works as follows. If any replica is down, the Coordinator writes to all the other replicas. But it also keeps a copy of the write locally and when that down replica comes back up, it is then sent a copy of that write



.



.However, all the replicas might be down,In this case instead of rejecting the write, the coordinator stores the write locally at itself, it buffers the writes and then when one or more of the replicas comes back up, it then relays the writes to the corresponding replicas. The coordinator of course has a time order on how it stores these writes, otherwise it may be storing writes forever. And this time order is typically a few hours, which is, good enough for replicas to recover and rejoin the system.



write and read support





when a client sends a write to a coordinator, The coordinator receives his write, uses the partitioner to find out which of the replicas that map to that particular key. He then sends the query to all the replica nodes responsible for that particular key.



Then some of the replicas respond, when X replicas respond, the coordinator returns an acknowledgement to the client that the write is done.





what does a replica server do when the coordinator forwards it a write? Well the first thing it does is that it

1.logs information this is used for failure recovery. So that if the replica server fails at any point of time here on. It can know by looking at the disk log after it recovers that there were some write that it completed only partially, and then it can ask the coordinator for information about this write.



After this the replica server updates a memtable. A memtable is an in-memory representation of multiple key-value pairs. Essentially a memtable maintains some of the latest key-value pairs that have been. Written at this particular replica server. It's a cache that can be searched by key. So you quickly search the particular key that you're trying to write. If it's present in the mem table, you update the corresponding value. If it's not present in the mem table, then you simply append to the mem table another key value pair.

This mem table is called a write back cache. Meaning that you're storing it temporarily and tentatively in a memory. Rather than writing directly to disk if you were writing directly to disk then it would be called write-through cache. And write-through cache is much slower than back cache is.



However the mem table has a maximum size and when this maximum size is reached, or when the mem table is very old then it is flushed to disk.

Essentially you create a, a an assist table or a sorted string table. Soon as you take the mem table and you sort all the keys within them so that the keys are all sorted and the values are present alongside the keys as well. And then is stored on a disc as an access table.

Now if you want to search for a particular key in the data file itself, in this data file that is the key value pairs, it might take a long time because it consists of both the keys and the values. So you maintain an index file as well, which stores an SSTable of key, comma, position in data SSTable pairs, so that it can quickly look up the position of a particular key in the data SSTable by looking at the index file SSTable itself.



And so what you do is you add a Bloom filter, which is a quick way of looking for whether or not a particular key is present in an SSTable.



merge the SSTable

over time a particular server might have a lot of SSTables that are written to disc. And a given key might be present in multiple SSTables. So over time you take the multiple SSTables that are present on disc, and you merge them. Essentially, you merge the, updates, for a given key. Okay? So if a key is present in two SSTables, you take the latest update for that key, and you replace, the older, the older update with that latest update.



The notion, the compaction, process is run, periodically, by each server. And it is run locally on the SSTables at that server.

Delete SSTable



Deletes. When you want to delete a particular key value pair, you don't need the item right away. Instead you write to the log or to the SSTable what is known as a tombstone. A tombstone is essentially a marker that says when you encounter this, please delete this particular key. Eventually when the compaction algorithm runs it encounters this tombstone and when it does so, it will delete the particular key value pair from the table itself.



Read



the client sends the read operation to the coordinator. The coordinator can contact a set of replicas. It doesn't necessarily contact all the replicas. It contacts only X replicas. X is the number, again, specified by the client.



Typically the coordinator might prefer replicas that have responded the quickest in the past. These are replicas for instance, that might be in the same rack as the coordinator, or that might be on nearby racks in the underlying data center topology itself.



Any of the X replicas when they respond, the coordinator can then return the latest time stamp value from among those X. When the different replicas for a given key might return different values, because we didn't say anything about consistency, which means that some of the replicas may have received, the latest write to that key. Other replicas may still be working with a stale value of the value for that particular key.



This means that when you get back answers from these X replicas for a read, some of them may be stale values, some of them may be newer values. So, essentially the coordinator looks at the timestamp of these values and the highest timestamp, or the latest timestamped value is returned to the client.

if any of these values are older, then the coordinator initiates a read repair on these older values. essentially, the read repair informs these older values, or rather, the replica servers that hey, here is the new and the latest value, the latest time stamped value that is associated with this particular key. Please update your corresponding mem table, and eventually SSTable. And if this goes on for long enough, this mechanism will eventually bring all the replicas for a given key up to date meaning that all of them will reflect the latest value that has been written to that particular key.



For reads however if compaction doesn't run often enough then a row maybe split across multiple SSTables.



consistency

So what is eventual consistency which is provided by Cassandra,



Eventual consistency essentially says the following:

Cassandra prefer available over consistency



some of the reads from clients may return staler values, older values that replicas.

when the writes continue coming, the system always try to updated values of the replicas or send the repaired message to update the value of replicas

the value of X is what is known as a consistency level in Cassandra. The client is allowed to choose the consistency level for each operation, a read or a write, for any key, uh, that it sends out. So for each operation that the client sends out, it can specify a given consistency level for that particular operation.



If the consistency level is ANY, this is an allowed consistency level by Cassandra, it means that any server can store that particular write and then return immediately to client. This is really useful because it's the fastest, even if all the replicas are down, the coordinator simply caches the write and returns immediately

The slowest, on the other hand, is the consistency level of ALL. This says that all the replicas if there are three replicas for a given key, then all the replicas need to acknowledge to the coordinator before the coordinator can say that the write has been done and return and acknowledgement the client,

