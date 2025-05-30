# Summary of Cache Consistency: Memcached at Facebook



---

the replica and sharding



we first request the data from cache layer, if it is missing, then we request the data from the database, then web service will update the value in the cache



the cache Facebook use is aside cache



if you update the data, the web service or front end will send a update to database and send a delete request to cache layer, the database also will send a invalid request to invalid that key.



make sure what the user read is what the user write.







face book has two region and both will handle the read request, but only the primary region will for the writing request, the change will forward to section region via log







there are 2 region and west coast and east coast, each region has a whole set of replica



Facebook actually uses a combination of both partition and replication



For the popular key, we can use multiple replica and they are going to store this not very popular key in the appropriate memcache server of the regional pool





one Storage cluster(houses backing store) and multiple frontend clusters(houses web server and memcache) are combined into a region; mcsqueal, are deployed on each database which parse the queries, extract and group delete statements and broadcast them to all the front end cluster in the region. The batched delete operations are sent to**mcrouter**instances in each frontend cluster, which then unpack the individual deletes and route them to the concerned Memcache server.


