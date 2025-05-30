# hbase



---

Hbase build on the hdfs

Hdfs is distribution file system base the idea of Google 's big table paper

Hbase split to different regions serves just like shading or partitioning



If the data growth it can automatically portion to the different regions serves



And those regions serves sitting on the hdfs

Service



If application service want to request the data they will get the address of region database from master node and talk to the region services directly



Hmaster or master node will track how stored or how partitioning the data

Zookeeper Wil watch the master node

If one master goes down zookeeper will find out who goes to next and tell everyone



Hbase has columns family and each column family have very larger number individuals column



Example of column family

The power of the column family is good to deal with the sparse data



For example we have a database to store the moves rating information



The se.. Is give a use id and we can look up the that user Id's rating very quickly



We don't need columns for each movie, we can create use hbase. So it is ok user don't have or rate each movie in the world.



**The row id the customer ID**

**Columns family is rating**

**Each column named rating:55 55 is the movie ID**

**And the value is the rating**








