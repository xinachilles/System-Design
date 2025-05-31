# Priority Queue



---

The application posting a message can assign a priority and the messages in the queue are automatically reordered so that those with a higher priority

will be received before those with a lower priority.





In systems that don't support priority-based message queues, an alternative solution is to maintain a separate queue for each priority.



The application is responsible for posting messages to the appropriate queue. Each queue can have a separate pool of consumers.



Higher priority queues can have a larger pool of consumers running on faster hardware than lower priority queues.





Or just have a single pool of consumers that check for messages on high priority queues first,

and only then start to fetch messages from lower priority queues.





In the single pool approach, higher priority messages are always received and processed before lower priority messages.



In theory, messages that have a very low priority could be continually superseded and might never be processed.



In the multiple pool approach, lower priority messages will always be processed, just not as quickly as those of a higher priority (depending on the relative size of the pools and the resources that they have available).





you have to provide a mechanism that can preempt and suspend a task that's handling a low priority message if a higher priority message becomes available.





It might be possible to scale the size of the pool of consumer processes handling a queue depending on the length of the queue.
