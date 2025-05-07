# ultimate

Created: 2019-06-11 23:53:12 -0600

Modified: 2019-06-11 23:53:37 -0600

---

Cassandra is no single point failure, there is no master node in Cassandra

The CAP theorem, CAP is standard forC is for consistency, a is for available and P is for partition tolerance and people provide by the paper that system only can have 2 of 3



The consistency is meaning if I wrote something or some datain the database, other users orI or user 'm going to get the data or answer back right away



The concept of eventual consistency, which is what Cassandra offers, meaning that if I write a value into Cassandra ,you might not get that value back right away, because it might take a few seconds to actually propagate that change throughout the entire cluster





maybe it's OK if that new value doesn't actually come back for a second or two.



The available is meaning you want your database to be reliable and always up and running



partition tolerance means that the database can be easily split up and distributed acrossa cluster,

it should always be true for large and distributed system. we really need distribute the data across the cluster



so the partition tolerance isnonnegotiable



the Cassandra is favor availability over the consistency. For Cassandra, it will give user the availability and partition tolerance and give up the consistency and actually deal with eventual consistency



for Cassandra, you can say how many nodes in my cluster need to agree on the result. If some nodes return the same answer. I will accept the result.



So it is really up to you as the a user of Cassandra how you want to make the choice.
