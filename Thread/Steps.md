1. Acquiring a lock 
	- acquire local lock -- ensure thread-safety within this JM
	- Create a distributed lock object and insert it into the MongoDB. MongoDB unique index on campaign name ensures only one lock document can exist per campaign across all services
	- ~~Added the campaign name in the ConcurrentSkipListSet~~
	- MongoDB + in-memory to ensure consistency 
 2. Renewing a lock 
	- Set up a scheduled task to renew every 60 s 
	 - Keep the lock still alive while the campaign is still running 
	 - MongoDB automatically deletes after TTL (TTL index on lastUpdated). Renewing updates the  lastUpdated timestamp to prevent expiration 
4. Removing a lock 
		- Acquire local lock 
		- Delete the lock from MongoDB 
		- remove from in-memory set (ConcurrentSkipListSet)

Why need 2 locks 
 1. ReentrantLock - Thread safety within one application instance 
 2. MongoDB Lock - All servers in the cluster , coordination across multiple services 