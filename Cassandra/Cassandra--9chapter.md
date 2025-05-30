# Cassandra  9chapter



---

![](../media/Cassandra-Cassandra--9chapter-image1.png)



![Row Key 又 称 为 Hash Key, Partition Key Cassandra 会 根 据 这 个 key 算 一 个 hash 值 然 后 决 定 整 条 数 据 存 储 在 哪 丿 L ](../media/Cassandra-Cassandra--9chapter-image2.png)



![Column Key insert(row_key, column_key, value) (fJ{EJ column_key Cassandra query(row_key, column_start, column_end) ](../media/Cassandra-Cassandra--9chapter-image3.png)



![• NoSQLfic01umn-EäjJäfi, grid row_key + column_key + value = • column _ key int int+string) NoSQL row_key I row_key2 column keyl value() column key2 column key3 value 1 column kev4 ](../media/Cassandra-Cassandra--9chapter-image4.png)



![Cassandra Friendship Table , row_key user idl ' column _ key <friend user id2> ' value <is mutual friend, is_blocked, timestamp> is <friend user id3> <is mutual friend, blocked, timestamp> is user id2 <friend user idl> <is mutual friend, blocked, timestamp> user id3 <friend user idl> <is mutual friend, is_blocked, timestamp> ](../media/Cassandra-Cassandra--9chapter-image5.png)



![(511+2 : Cassandra NewsFeed , row_key ' column _ key <crea e ' value owner id2 owner id3 a owner idl <crea e a <crea e a <crea e a <tweet datal> <tweet data2> <tweet data3> <tweet data4> ](../media/Cassandra-Cassandra--9chapter-image6.png)








