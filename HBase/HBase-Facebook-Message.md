# HBase Facebook Message 



---

![üip slide When to use HBase? • storing large amounts of data • need high write throughput • need efficient random access within large data sets • need to scale gracefully with data • for structured and semi-structured data • don't need full RDMS capabilities (cross table transactions, joins, etc.) ](../media/HBase-HBase-Facebook-Message-image1.png)



strong consistency



![HBase Data Model An HBase table is: a sparse , three-dimensional array of cells, indexed by: RowKey, ColumnKey, Timestamp/Version sharded into regions along an ordered RowKey space Within each region: Data is grouped into column families Sort order within each column family: Row Key (asc), Column Key (asc), Timestamp (desc) ](../media/HBase-HBase-Facebook-Message-image2.png)



![Example: Inbox Search Schema Key: RowKey: userid, Column: word, Version: MessagelD Value: Auxillary info (like offset of word in message) Data is stored sorted by <userid, word, messagelD>: Userl Userl Userl 6->0ffset3 Userl :hello:2->0ffset4 User2:..._ User2:... Can efficiently handle queries like: - Get top N messagelDs for a specific user & word - query: for a given user, get words that match a prefix ](../media/HBase-HBase-Facebook-Message-image3.png)





