
---

**Rate Limiter**

**Function**

Limit the number of requests for each Customer or IP address within a time window, e.g., 15 requests per second for Customer A.

**QPS**

Or how many users use this system

Or what'`s limitation (Let's suppose we need a rate limiting of 500 requests per hour)

**High-level design**

Once a new request arrives, the Web Server first asks the Rate Limiter to decide if it will be served or rejected. If the request is not rejected, then it'll be passed to the API servers.

**Storage**:

**For the fix windows algorithm**

We want to limit the number of requests per user. We would keep a count representing how many requests the user has made and a timestamp regarding the requests.

We can keep it in a hashtable, where the 'key' would be the 'UserID' and 'value' would be a structure containing an integer for the 'Count' and an integer for the Epoch time:

Epoch time is the number of second from current time to 1970, 1 January. It is 32 bit or 4 bytes.

Let's assume 'UserID' takes 8 bytes. Let's also assume a 2 byte 'Count', which can count up to 65k, is enough for our use case.

Hence, we need a total 14 bytes to store a user's data:

4+8+2 = 14

Let's assume our hash table has an overhead of 20 bytes for each record. If we need to track **1 million** users at any time, the total memory we would need would be

32MB:

(14 + 20) bytes * 1 million => 34MB



If we assume that we would need a 4-byte number to lock each user's record to resolve our atomicity problems, we would require a total of 36MB of memory.

**Sliding windows**

Let's assume 'UserID' takes 8 bytes. Each epoch time will require 4 bytes. Let's suppose we need a rate limiting of 500 requests per hour. Let's assume 20 bytes of overhead for hash-table and 20 bytes of overhead for the Sorted Set. At max, we would need

a total of 12KB to store one user's data:

8 + (4 + 20 (sorted set overhead)) * 500 + 20 (hash-table overhead) = 16KB

Here are reserving 20 bytes overhead per element. In a sorted set, we can assume that we need at least two pointers to maintain order among elements. One pointer to the previous element and one to the next element.

On a 64bit machine, each pointer will cost 8 bytes. So we will need 16 bytes for pointers. We added an extra word (4 bytes) for storing other overhead.

If we need to track one million users at any time, total memory we would need would be 12GB:

12KB * 1 million ~= 12GB

**Sliding windows with count**

How much memory we would need to store all the user data for sliding window with counters? Let's assume 'UserID' takes 8 bytes. Each epoch time will need 4 bytes and the Counter would need 2 bytes. Let's suppose we need a rate limiting of 500 requests per hour. Assume 20 bytes overhead for hash-table and 20 bytes for Redis hash.

Since we'll keep a count for each minute, at max, we would need 60 entries for each user. We would need a total of 1.6KB to store one user's data:

8(user id) + (4 + 2 + 20 (Redis hash overhead)) * 60 + 20 (hash-table overhead) = 1.6KB

If we need to track one million users at any time, the total memory we would need would be 1.6GB:

1.6KB * 1 million ~= 1.6GB

So, our 'Sliding Window with Counters' algorithm uses 86% less memory than simple sliding window algorithm

What if we keep track of request counts for each user using multiple fixed time windows, For example, if we've an hourly rate limit, we can keep a count for each minute and calculate the sum of all counters in the past hour when we receive a new request to calculate the throttling limit.

It also sets the hash to expire an hour later. We'll normalize each 'time' to a minute.

**Data Sharding and Caching**

This can easily fit on a single server, however we would not like to route all of our traffic through a single machine. Also, if we assume a rate limit of 10 requests per second, this would translate into 10 million QPS for our rate limiter! This would be too much for a single server.

We can shard based on the 'UserID' to distribute user's data. For fault tolerance and replication we should use Consistent Hashing.

**Persistency Storage**

**If we just need a support rate limit per hour, we don`t need



Redis supports persistency in memory storing or persistent storing (per

AOF is append only log

Snapshot

Token bucket is an algorithm for global requests. The basic idea is that there are four global variables.

Last request time, capacity, tokens per second, and current Taken.

First, we need to figure out capacity based one the unit, the unit can be seconds, minute, or hours

We will increase the taken base on the current time and last request time

The maximum taken they can have can not be more than the capacity.

If the taken less than 0, reject the request

Otherwise, taken the request and the taken mins one

<http://blog.gssxgss.me/not-a-simple-problem-rate-limiting/>

![image.png](https://i.imgur.com/jiXEBCh.png)




