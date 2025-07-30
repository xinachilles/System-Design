
<https://www.youtube.com/watch?v=UesxWuZMZqI>

### What is a Message?
- A message is data sent between software components (higher-level than an IP packet), often in formats like JSON.
- Messages have a payload (the main data) and attributes (key-value pairs for filtering/routing).
- Payloads can be encrypted; attributes usually remain unencrypted.

### Queues
- Queues are durable buffers for messages, decoupling producers and consumers.
- Producers send messages to the queue; consumers pull and acknowledge messages, which then get removed.
- Queues help handle spikes in traffic and prevent backend overload.

### Message Ordering & Groups
- Strict ordering (FIFO) requires a **single worker** per stream.
- To scale, use message groups: messages in the same group are processed in order, but different groups can be processed in parallel (e.g., by session ID).
-for kakfa, if there are mutiple consumers in the same group, can we guareteen we consume the message in order

-One example of where you might want to do that is if you're tracking website with a huge number of users interacting with the website so you would just use the session ID as a message group and that way each individual users session transactions would be processed properly in order, but you can still handle a huge number in parallel interleaved with no problem

**Explanation:**

- Imagine a website where many users are active at the same time.
- Each user’s actions (like clicks, purchases, etc.) are grouped by their session ID.
- By using the session ID as a "message group," you ensure that all messages (actions) from the same user are processed in the correct order.
- At the same time, because each user has a different session ID, the system can process messages from different users in parallel.
- This means you maintain order for each user’s actions, but the system can scale to handle many users at once, efficiently processing lots of activity without mixing up the order of actions within a single user’s session.

**In short:**  
Grouping messages by session ID lets you process each user’s actions in order, while still allowing the system to process many users’ actions at the same time.

### Publish-Subscribe (Topics)
- In pub/sub, each subscriber receives a copy of every message published to a topic.

### Message Streams
- Streams are ordered logs of messages, persistent for a set time.
- Consumers use cursors to read messages; messages aren’t deleted after reading.
- Use streams for analytics or when multiple consumers need to process the same data.

### When to Use Queues vs. Streams
- Use queues for independent, one-time processing (message deleted after consumption).
- Use streams for sequential analysis or multiple consumers.

### Architectural Benefits
- Queues buffer requests, allowing backend services to process at their own pace and preventing overload.
- Asynchronous background tasks should use queues for reliability (work isn’t lost if a worker fails).

### Queue Configuration
- Prefer a shared queue for background tasks (any worker can process), rather than a queue per worker.

### Decoupling with Topics
- Use topics to decouple services: the producer sends to a topic, and all subscribers (e.g., notification, finance, partners) receive the message.
- This makes it easy to add new integrations and manage retries/failures.

### Long Polling
- Long polling allows consumers to wait for messages efficiently, reducing the need for constant polling and enabling near-instant message delivery.

### Security
- Support for server-side encryption of messages.

---


































Because there are huge number of web service. they can sent the huge request to the second layer, the booking service is not prepare to handle that load. what's can go wrong is your relational DB can start to failure some request and time out them. Which turns out is you prepare turn 0 result back to your customer



One solution is you can replace the load balance with queue. the booking service connected to the database can consumer the message in the queue with the rate they can comfortable with, the database can keep up with. this can protect from the capacity mismatch between the layers in your service, one layer can spike up traffic in other one cannot handle it.



when you put the message into the queue, it is durable, it's not going way. You can count on it







(20 mintes)

person is actual book a hotel he has to wait up the 3 second

the actual database operation is faster but extra steps taking a long time



so one of the things that you could do is okay let's break it down



let's just launched a separate thread background said to do the extra processing in the background



It's all nice but the problem is if it's just a background siting in the exist process what will happen if your job a process goes down because the machine had a problem or there were other issues physically



You now have lost work you already starting the database of the booking have been done but all the extra work has not been processed yet so you can have lost work



it's better to just use a queue for a asynchronous tasks. You mark the book completed in DB and stored the extra work in the queue


You have a background work that keeps receiving tasks from the worker queue and processing all extra work then delete the message is done. so even the work dies and when you resume the operations are still a message in the queue complete the work.



what's the limitation of the message size : a quarter of a megabyte 256 kb



how should I configure my background queue.





One approaches is to have a Q per worker this is a little bit risky because if a worker goes down there's no one to process the items from the queue.

There are still a nice option if there is a asynchronous task you need complete, need access to local resources on the booking service host, for instance on some file only that machine has, the background task need access those files.





~~Eve the icing from Eustis that you need to complete needs access to the local resources on that broker on the booking service house~~



~~For instance there are some files on this visit only this machine has and the background task needs to access those files with~~



My recommended approach would be something like this but do you have a single queue for background task and any worker can and process the background tasks and completed.



The third example, you've done all the work and you building a model service



You broken down the responsibilities into mico service so now you have multiple services are interconnected to process the hotel booking



When a booking happens if you need to notify and Lambda function that will email the notification about the booking has been made



You make a call to your booking partner that has been made you send a message to a finance queue and so on. In this scenario you have to implement the code yourself to talk to all of your parties



It makes it really hard to add the 5th or 6th or 7th integration at this point because it's you that have to have to go back and modify the booking service go and there's an extra complications like what should you do.



If you fail to call the external partner and you need keep retrying it or fail. How can you roll back like those out of the hard questions that you would have to solve in your own call.





~~in particular what happens if you get settled this happens when you talk to call me back and you're going to potentially end up dealing with multiple so I don't see how it goes at once and parallel was to lead to some pretty nasty code so~~





The nice way to the couple the booking service on its dependencies is to introduce a topic in the middle.



~~you can figure it out once you subscribe the lamp. Email notification service just once you subscribe and HDPE subscription for the external fire department division just one through~~



just configured ones and now whenever a booking is being made through your service you probably should do a topic. Not call immediately completes like it's very low latency, you booking service get OK



I think the SMS will make sure those notification will be delivered to all of your subscribers



~~even for like for Integrations as SQS with asked you asked as soon as~~ we will always deliver messages. We will keep trying until it succeeds. For integration with HTTP endpoint which I like software that you are running or your partners are running.



You are you can configure I retry policy how long the call should be retried and what range the deliveries should be attempted and so on and you will also can be notified if the HTTP endpoint actually failed



All the retry policy get failed and you will get a notification about the failed delivery



~~so we talked about this business and there's more features in both services.~~



we support long falling what is Long poling, imagine you have a Q and you have an API to access this queue. I want to process the messages as fast as they arrived.



If you have simple receive call. This means you will have to keep calling receive in a very tight Loop never pausing because you want to get the messages fast as you can.



The opposite would be kind of push delivery. You would say okay if there's something in the queue please call me back and let me know so I can actually process the message.



That's risky because if there's huge traffic going to the queue. You will be overloaded by those notifications about messages in the queue.



So long pulling is actually the proper solution to this problem and this means that you make a receive call to the queue. You're saying I'm willing to wait after like 20 seconds to get a new message and you keep waiting and if the queue get the message. The response will push to you over this long pull. receive call immediately.



so it's very cheap because the connections that you're making are like once they're 20 seconds and you get instant pushes of messages when they arrived into the Q.



We also support service site encryption and it's very easy and convenient
