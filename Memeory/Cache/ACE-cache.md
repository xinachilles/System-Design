# ACE cache

Created: 2021-04-26 10:26:59 -0600

Modified: 2022-02-18 13:04:40 -0600

---

Amazon Training

**Read strategy**



![Clllt Cache-Aside 1. Cache hit: read fro che Cache e data to cache 2. Cache miss: read from database Application Database ](../../media/Memeory-Cache-ACE-cache-image1.png){width="5.0in" height="3.2291666666666665in"}

Cache don't need to talk to database



![Read-Through l. lit: Read from cache Application Cache 2. Read from and store in cache Databa ](../../media/Memeory-Cache-ACE-cache-image2.png){width="5.0in" height="2.34375in"}

Cache will talk to database





Write strategy



![Write-Through Write to cache Application Write to database Cache Databa ](../../media/Memeory-Cache-ACE-cache-image3.png){width="5.0in" height="2.21875in"}



Write successfully when write into cache and database ( update cache and database --- consistency between .. )

![Write-Back Write to cache Application Cache Write to database Databae ](../../media/Memeory-Cache-ACE-cache-image4.png){width="5.0in" height="2.1041666666666665in"}



Write will return successful when data write into cache, cache will flush into databse when cache is full

![2. Who can see your viewing activity? Lazy Loading def fetch(sql): key=get_md5_hash(sqL) if get(key) is not None r. pickle . Loads(va Lue) return else: o cursor=m. cursor =executeCsqL) cursor value---cursor . fetcha L L ( ) setex(key, T L, pickle . dumps(va Lue)) r. value return 1. Read from cache 2. Read from source (if miss) 3. Write to cache ](../../media/Memeory-Cache-ACE-cache-image5.png){width="5.0in" height="2.5520833333333335in"}
1.  Lazing loading -- similar to Cache aside



the advantage

<https://indeed.zoom.us/rec/play/H3XLrrAdLHlr4anw0gvt7RENPqJ2EY4ilBNubGISmNL8ouFNLKxGYrFXybXYVgrLHBPW1P1scxpy8usZ.EsVVOADAIn8Uzso3?continueMode=true>

00:56



avoid some un-necessary data in the cache

Cache can be repopulated at anytime immediate



Disadvantage:

The cache miss could be expensive. If there is no data in the cache, you need read the data from db and update the data to the cache as well

![Lazy Loading Clients Amazon EC2 AWS Lambda 3 - Write 1 - Read 2 - Read Who can see your viewing activity? Amazon ElastiCache for Redis Amazon RDS 1. Read from cache 2. Read from source (if miss) 3. Write to cache Advantages Avoids unnecessary data in cache Cache can be repopulated at anytime Immediate benefit Disadvantages Cache miss may be expensive Data "freshness" factor aws @ 2021, Amazon Web Services, Inc. or its Affiliates. ](../../media/Memeory-Cache-ACE-cache-image6.png){width="5.0in" height="2.7395833333333335in"}



![Write-Through Example 2 Clients Amazon EC2 AWS Lambda 1 2 - Write - Write Who can see your viewing activity? Amazon ElastiCache for Redis Amazon RDS 1. Write to source 2. Write to cache Advantages Data is never stale Write latency better tolerated by customers vs. read latency Disadvantages Unnecessary data, cache churn Delayed vs. immediate benefit aws @ 2021, Amazon Web Services, Inc. or its Affiliates. ](../../media/Memeory-Cache-ACE-cache-image7.png){width="5.0in" height="2.7083333333333335in"}
1.  Client don't need to deal with the write to cache
2.  Need to wait longer for writing
3.  Unnecessary data in the cache: if you've never read the data from cache then the data will still in the cache
4.  Delay: you have to wait lamdba write the data to the cache



Update database

Update cache



![Write-Through - Python example update _ Location( "mike " userid:mike "DFW" first team location Michael ElastiCache DEW def update _ Location(name, new _ Loc): # Write Through - Step 1: Update DB cursor=m. cursor sql = IUPDATE.. .WHERE userid (name) (sql) cursor. execute db . commit o # Write Through - hset( "userid: r. Step 2: Update Cache (name) , "location' new _ Loc) ](../../media/Memeory-Cache-ACE-cache-image8.png){width="5.0in" height="2.4583333333333335in"}

<https://indeed.zoom.us/rec/play/H3XLrrAdLHlr4anw0gvt7RENPqJ2EY4ilBNubGISmNL8ouFNLKxGYrFXybXYVgrLHBPW1P1scxpy8usZ.EsVVOADAIn8Uzso3?continueMode=true>

![2. Who can see your viewing activity? Caching Strategies - Lazy Load Only caches data that is read ummary Cache populates when reads occur Immediate benefit Latency from cache miss is expensive Data can get stale Often uses String * Write-Through Caches data that may never be read Cache populates when updates occur Delayed benefit Write latency better tolerated vs. read latency Data is never stale Often uses Hash * You can combine these strategies! * Commonly used but not limited to this data type ](../../media/Memeory-Cache-ACE-cache-image9.png){width="5.0in" height="2.65625in"}

![Write-Around Cache Write to DB Application Database ](../../media/Memeory-Cache-ACE-cache-image10.png){width="5.0in" height="3.0208333333333335in"}

Or write through pattern

Client don't need to deal with the updating the cache





![](../../media/Memeory-Cache-ACE-cache-image11.png){width="5.166666666666667in" height="0.5416666666666666in"}



![Cache-Aside Write-Through N/A Worst Write Read-Through Best Read Consistent Write-Around Simple Space Efficient Inconsistent Less Latency Space Efficient Inconsistent PW t Trie* Dong write Write-Bac Allen N/A To: write---b. Best Write Best Read Inconsistent ](../../media/Memeory-Cache-ACE-cache-image12.png){width="5.0in" height="2.8854166666666665in"}

Cache - aside / write around need a TTL



![Redis 永 久 化 方 案 Mic 实 訓 大 Alle 全 称 优 点 Redis Database 文 件 小 ， 不 影 响 读 写 速 度 ， 快 速 灾 后 修 RDB Backup File Append-only 最 大 程 度 降 低 数 据 丢 失 ， 记 录 每 次 读 写 AOF File ](../../media/Memeory-Cache-ACE-cache-image13.png){width="5.0in" height="2.5208333333333335in"}

RDB -- snapshot

















