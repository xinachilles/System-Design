# Rate Limter2



---

**Rate Limiter**

**Function**

Limit the number of requests for each Customer or IP address within a time window, e.g., 15 requests per second for Customer A.

**QPS**

or how many user use this system

or what`s limitation (Let's suppose we need a rate limiting of 500 requests per hour)

**High level design**

Once a new request arrives, Web Server first asks the Rate Limiter to decide if it will be served or rejected. If the request is not rejected, then it'll be passed to the API servers.

Storage:

**For the fix windows algorithm**

We want to limit the number of requests per user. We would keep a count representing how many requests the user has made and a timestamp regarding the requests.





In a **fixed window** algorithm, a window size of n seconds (typically using human-friendly values, such as 60 or 3600 seconds) is used to track the rate. Each incoming request increments the counter for the window. If the counter exceeds a threshold, the request is discarded. The windows are typically defined by the floor of the current timestamp, so 12:00:03 with a 60 second window length, would be in the 12:00:00 window.

We can keep it in a hashtable, where the 'key' would be the 'UserID' and 'value' would be a structure containing an integer for the 'Count' and an integer for the Epoch time:

{User ID -> pair <Start_time, count}

Let's assume our rate limiter is allowing three requests per minute per user, so whenever a new request comes in, our rate limiter will perform following steps:



If the 'UserID' is not present in the hash-table, insert it and set the 'Count' to 1 and 'StarteTime' to the current time (normalized to a minute-- epoch time) , and allow the request.



If find the record of the 'UserID' and if 'CurrentTime -- StartTime >= 1 min', [reset]{.mark} the 'StartTime' to the current time and 'Count' to 1, and allow the request.



If 'CurrentTime - StartTime <= 1 min' and

If 'Count < 3', increment the Count and allow the request.

If 'Count >= 3', reject the request.



[Storage for fixed window]{.mark}

The start time is a epoch time

Epoch time is the number of second from current time to 1970, 1 January. [It is 32 bit or 4 bytes]{.mark}.

Let's assume 'UserID' takes 8 bytes. Let's also assume a 2 byte 'Count', which can count up to 65k, is enough for our use case.

Hence, we need total 14 bytes to store a user's data:

4+8+2 = 14

Let's assume our hash-table has an overhead of 20 bytes for each record. If we need to track **1 million** users at any time, the total memory we would need would be

32MB:

(14 + 20) bytes * 1 million => 34MB

If we assume that we would need a 4-byte number to [lock]{.mark} each user's record to resolve our atomicity problems, we would require a total 36MB memory.

Problem:

This is a Fixed Window algorithm, as we're resetting the 'StartTime' at the end of every minute, [which means it can potentially allow twice the number of requests per minute.]{.mark} Imagine if Kristie sends three requests at the last second of a minute, then she can immediately send three more requests at the very first second of the next minute, resulting in 6 requests in the span of two seconds.

In a distributed environment, the "read-and-then-write" behavior can create a race condition. Imagine if Kristie's current 'Count' is "2" and that she issues two more requests. [If two separate requests read the Count before either of them updated it,]{.mark} each process would think the Kristie can have one more request and that she had not hit the rate limit.

**Sliding windows**

Let's assume our rate limiter is allowing three requests per minute per user, so whenever a new request comes in the Rate Limiter will perform following steps:

1. Remove all the timestamps from the Sorted Set that are older than "CurrentTime - 1 minute".

2. Count the total number of elements in the sorted set. Reject the request if this count is greater than our throttling limit of "3".

3. Insert the current time in the sorted set, and accept the request.





Let's assume 'UserID' takes 8 bytes. Each epoch time will require 4 bytes.

we need a rate limiting of 500 requests per hour. Let's assume 20 bytes overhead for hash-table and 20 bytes overhead for the Sorted Set. At max, we would need a total of 12KB to store one user's data:

8 + (4 + 20 (sorted set overhead)) * 500 + 20 (hash-table overhead) = 16KB

Here are reserving 20 bytes overhead per element. In a sorted set, we can assume that we need at least two pointers to maintain order among elements. One pointer to the previous element and one to the next element.

On a 64bit machine, each pointer will cost 8 bytes. So we will need 16 bytes for pointers. We added an extra word (4 bytes) for storing other overhead.

If we need to track one million users at any time, total memory we would need would be 12GB:

12KB * 1 million ~= 12GB



**Sliding windows with count**

if we've an hourly rate limit, we can keep a count for each minute and calculate the sum of all counters in the past hour

Let's assume 'UserID' takes 8 bytes. Each epoch time will need 4 bytes and the Counter would need 2 bytes. Let's suppose we need a rate limiting of 500 requests per hour. Assume 20 bytes overhead for hash-table and 20 bytes for Redis hash.

Since we'll keep a count for each minute, at max, we would need 60 entries for each user. We would need a total of 1.6KB to store one user's data:

8(user id) + (4 + 2 + 20 (Redis hash overhead)) * 60 + 20 (hash-table overhead) = 1.6KB

If we need to track one million users at any time, total memory we would need would be 1.6GB:

1.6KB * 1 million ~= 1.6GB

So, our 'Sliding Window with Counters' algorithm uses 86% less memory than simple sliding window algorithm

What if we keep track of request counts for each user using multiple fixed time windows, For example, if we've an hourly rate limit, we can keep a count for each minute and calculate the sum of all counters in the past hour when we receive a new request to calculate the throttling limit.

it also sets the hash to expire an hour later. We'll normailze each 'time' to a minute.

**Data Sharding and Caching**

This can easily fit on a single server, however we would not like to route all of our traffic through a single machine. Also, if we assume a rate limit of 10 requests per second, this would translate into 10 million QPS for our rate limiter! This would be too much for a single server.

We can shard based on the 'UserID' to distribute user's data. For fault tolerance and replication we should use Consistent Hashing.

**Persistency Storage**

**If we just need support rate limit per hours, we don`t need**

Redis support persistency :in memory storing or persistent storing (per

AOF append only log

Snapshot

Token bucket is a algorithm for the global request. The basic ideal is there are four global variable.

Last request time, capacity, tokensPerSeconds and current Taken.

First, we need figure out capacity base on the unit, the unite can be second , minute or hours

We will increase the taken base on the current time and last request time

The maximum taken cannot more than the capacity.

If the taken less than 0, reject the request

Otherwise, taken the request and the taken mins one

<http://blog.gssxgss.me/not-a-simple-problem-rate-limiting/>

private final int capacity;

private final int tokensPerSeconds;

private int tokens = 0;

private long timestamp = System.currentTimeMillis();

public TokenBucket(int tokensPerUnit, TimeUnit unit) {

capacity = tokensPerSeconds = (int) (tokensPerUnit / unit.toSeconds(1L));

}

public boolean take() {

long now = System.currentTimeMillis();

tokens += (int) ((now - timestamp) * tokensPerSeconds / 1000);

if (tokens > capacity) tokens = capacity;

timestamp = now;

if (tokens < 1) return false;

tokens--;

return true;

}

}

public static void main(String[] args) throws InterruptedException {

TokenBucket bucket = new TokenBucket(250, TimeUnit.MINUTES);

Thread.sleep(1000L);

for (int i = 0; i < 5; i++) {

System.out.println(bucket.take());

}

Thread.sleep(1000L);

for (int i = 0; i < 5; i++) {

System.out.println(bucket.take());

}

}


