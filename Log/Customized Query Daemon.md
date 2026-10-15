#### Spark job on EMR 
- Build an inverted index for the Query Daemon 
	- ##### How the log file is stored	 
		- Build the inverted index and store it in HDFS/S3 as a Squall Archives file (or directory)
		    - Each dataset (folder) has shards (time-ranged partitions). the file name like log20231031.20231031082156.sqar
			(log+date+build time)
		- Each folder/directory contains a metadata file and one or more table data files. 
		- Metadata has each file size, checksum, **filename** (= table name), and **offset**
		- Table data files are compressed and concatenated into a single file. 
		- There are 3 table data files: terms file, doc file, and B-tree index file
			- Terms file: each entry contains 2 pieces:  
				- Term or term delta ( the number of delta characters from the first term)
				- Doc frequency: how many docs have this term
			- Doc file: 
				- Each entry contains doc ids  
				- Delta - encoded: 1, +4, +2, each number is the delta from the previous
				- doc ids are sorted, and if the "where" condition contains multiple terms, the result should be the intersection of two entries.   
			- B tree index file: 
				- each term has (doc file offset + doc id offset) (100, 200)
				- Jump to byte 100 in the doc file
				- Read the doc id starting from 200

#### Query Daemon:
- Service or UI that enables querying the log file 
- ###### Query 
	  `select count()`
	 `from table_name` 
	 `where day>= xx and day <= xx`
 
 Complete Query Flow: 
- Parse query  
- Based on the query, the **shardmaster** says which shards or folders to query
- Checks cache (HDFS/Redis)
- Query Daemon opens the shard and retrieves the metadata file to obtain the file name, size, and offset.
- Extract and decompress the file 
-  Query the B-tree index for (doc file offset and doc id offset), terms file for the doc frequency, and doc files for the count of docs
- Return the result

    
  Can put the entire folder or Squall Archives file  into the cache





##### Shard/master service
- Track shard locations across the query daemon
- Store the shard locations in the DB  
	- shard name, shard start time, shard end name, hdfs path, assigned_daemon, status
- Update/refreshes from HDFS every minute
- Use **ZooKeeper** leader selection (high availability requirements)
	- The leader shard master does:
		- Scan the HDFS/s3 for the new shards (every 1 minute)
		- Write the new shard information into the database 
		- Delete the stale shards 
		- Monitor the [[Customized Query Daemon#Query Daemon]] health 
		- The leader cannot scan HDFS without Zookeeper
	- The follower shard master does 
		- Scan HDFS for all shards/datasets every 1 minute
			- Query the database for shard information 
			- Load the shard and assign it to the cache 
		- Serve the [[Customized Query Daemon#Query Daemon]] request (like what shards exist for xx yesterday) (look up in memory cache and SQL) 
		- Watch for leader failure 
- Register/unregister daemons


##### Zookeeper
- The zookeeper will select one [[Customized Query Daemon#Shard/master service]] as the leader that performs certain critical operations, while the others act as passive "followers" (read-only mode)
-  New query daemons cannot register without Zookeeper 