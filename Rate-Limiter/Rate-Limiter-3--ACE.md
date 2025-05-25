**Rate Limiter**

**Function**

Limit the number of requests for each Customer or IP address within a time window, e.g., 15 requests per second for Customer A.

Limit how often each user calls the API

**QPS**

Or how many users use this system

Or what'`s limitation (Let's suppose we need a rate limiting of 500 requests per hour)

**High-level design**

Once a new request arrives, the Web Server first asks the Rate Limiter to decide if it will be served or rejected. If the request is not rejected, then it'll be passed to the API servers.

**Algorithm**

![image.png](https://i.imgur.com/MRndT81.png)



The fixed window counter algorithm divides the timeline into fixed-size windows and assigns a counter to each window. Each request, based on its arrival time, is mapped to a window. If the counter in the window has reached the limit, requests falling in this window should be rejected.

![image.png](https://i.imgur.com/u4zpIvC.png)



Problem

This is a Fixed-Window algorithm, as we reset the 'StartTime' at the end of every minute, which means it can potentially allow twice the number of requests per minute. Imagine if Kristie sends three requests at the last second of a minute, then she can immediately send three more requests at the very first second of the next minute, resulting in 6 requests in the span of two seconds.

![image.png](https://i.imgur.com/YDQu7xl.png)




<https://hechao.li/2018/06/25/Rate-Limiter-Part1/>

**Sliding windows (sliding log)** involves tracking a time stamped log for each consumer's request. These logs are usually stored in ~~a hash set or table~~ / queue that is sorted by time. Logs with timestamps beyond a threshold or boundary are discarded. When a new request comes in, we calculate the sum of logs to determine the request rate. If the request would exceed the threshold rate, then it is held.

![image.png](https://i.imgur.com/qgY5iFR.png)
**Sliding windows with count**

Sliding window counter is similar to fixed window counter

, but it smooths out bursts of traffic by ==adding a weighted count in the previous window to the count in the current window==.

For example, suppose the limit is 10 per minute. There are 9 requests in window ==(00:00, 00:01)== and 5 reqeuests in window ==(00:01, 00:02)== For a requst arrives at ==00:01:15==, which is at ==25%== position of window ==(00:01, 00:02)==, we calculate the request count by the formula: 9 x (1 - 25%) + 5 = 11.75 > 10. Thus, we reject this request. Even though both windows don't exceed the limit, the request is rejected because the weighted sum of the previous and current windows exceeds the limit.

![image.png](https://i.imgur.com/qlaB3MO.png)
![image.png](https://i.imgur.com/73YLOhW.png)
<https://preparingforcodinginterview.wordpress.com/2020/06/20/designing-an-api-rate-limiter/>

we track a counter for each fixed window. Next, we account for a weighted value of the previous window's request rate based on the current timestamp to smooth out bursts of traffic. For example, if the current window is 25% through, then we weight the previous window's count by 75%.

**Leaky bucket**

[Leaky bucket](https://en.wikipedia.org/wiki/Leaky_bucket) (closely related to [token bucket](https://en.wikipedia.org/wiki/Token_bucket)) is implemented via a queue (or not), which you can think of as a bucket holding the requests. When a request is registered, it is appended to the end of the queue. The queue is a first in first out (**FIFO**) queue. If the queue is full, then additional requests are rejected

The request will be passed or leaked from the beginning of the queue at a certain rate

The input rate can vary, but the output rate remains constant

The advantage of this algorithm is saves bursty traffic into fixed-rate traffic by averaging the data rate.

It's also easy to implement on a single server or load balancer, and is memory efficient for each user given the limited queue size.

However, a bursty of traffic can fill up the queue with old requests and starve (or reject) more recent requests from being processed. It also provides no guarantee that requests get processed in a fixed amount of time.

![image.png](https://i.imgur.com/PQk1FH0.png)

![image.png](https://i.imgur.com/8aCDlVJ.png)

![image.png](https://i.imgur.com/9vMiU6N.png)


The requests are processed at an approximately constant rate, which smooths out bursts of requests. Even though the incoming requests can be burst, the outgoing responses are always at the same rate.

![image.png](https://i.imgur.com/3n6wW45.png)
**Token bucket**

![Screenshot 2025-05-24 at 10.05.21 PM.png](https://i.imgur.com/1tD07oB.png)




![image.png](https://i.imgur.com/UXMJc8Y.png)
![image.png](https://i.imgur.com/oqmgoQ5.png)
![image.png](https://i.imgur.com/7WwoqVb.png)


The difference between the token bucket and the Leaky bucket

If a constant rate

Token bucket allows bursts to be sent faster rate after that constant rate

The leaky bucket sends the packet at a constant rate

**(** <https://konghq.com/blog/how-to-design-a-scalable-rate-limiting-algorithm/>)

Token bucket, the request processing rate is not capped, which means it only guarantees the average processing rate will not exceed the maximum rate

We recommend the **sliding window** approach because it gives the flexibility to scale rate limiting with good performance.

It also avoids the starvation problem of the leaky bucket and the bursting problems of fixed window implementations.

![image.png](https://i.imgur.com/88znw8t.png)

![image.png](https://i.imgur.com/fTgGJoR.png)

![image.png](https://i.imgur.com/hNIuLFH.png)<https://konghq.com/blog/how-to-design-a-scalable-rate-limiting-algorithm/>

**Each client has a bucket**

**Sticky session**

Services need to remember for each client, how many tokens are left in his bucket. We can store this information in the servers, each client only go to that specific servers.

We set up sticky sessions in your load balancer, so that each consumer gets sent to exactly one node or service. The disadvantages include a lack of fault tolerance and scaling problems when nodes get overloaded.

Or we can user centralized store like redis : client id -> number of token in his bucket

Client id ->token count (and last refill timestamp)

Problem: race condition in high-concurrency requests

This issue happens when you use a naïve "get-then-set" approach, wherein you retrieve the current rate limit counter, increment it, and then push it back to the data store. This model's problem is that additional requests can come through in the time. It may read the old data or invalidate an invalid (lower) counter value. This allows a consumer to send a very high rate of requests to bypass rate-limiting controls.

One way to avoid this problem is to put a "**lock**" around the key in question, preventing any other processes from accessing or writing to the counter. A lock would quickly become a significant performance bottleneck and does not scale well.

A better approach is to use a "**set-then-get**" mindset, relying on atomic operators-- set then get is an atomic operation

**Near real-time check**

The increased latency is another disadvantage of using a centralized data store when checking the rate limit counters.

Unfortunately, even checking a fast data store like Redis would result in milliseconds of additional latency for every request.

Make checks locally **in memory** to make these rate limit determinations with minimal latency. To make local checks, relax the rate check conditions and use an ==eventually consistent model==.

For example, each node can create a data sync cycle that will synchronize with the centralized data store. Each node periodically pushes a counter increment for each consumer and window to the datastore. These pushes atomically update the values. The node can then retrieve the updated values to update its in-memory version. This cycle of converge → diverge → reconverge among nodes in the cluster is eventually consistent.

~~The periodic rate at which nodes converge should be configurable. Shorter sync intervals will result in less divergence of data points when spreading traffic across multiple nodes in the cluster (e.g., when sitting behind a round robin balancer). Whereas longer sync intervals put less read/write pressure on the datastore and less overhead on each node to fetch new synced values.~~
![image.png](https://i.imgur.com/a9D7y16.png)
![image.png](https://i.imgur.com/5wa51gX.png)

1.  Request to go to the rate limiter server
2.  Rate limiter server checks the configuration file or service to find out the limitation of the request per minute or per second
3. Check the redis to find out how many tokens this client has left
4. Decide pass or reject this request
![image.png](https://i.imgur.com/ypVea6q.png)



![image.png](https://i.imgur.com/K4ZaCcj.png)




















