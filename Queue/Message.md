# Message 

Created: 2019-11-11 12:22:55 -0600

Modified: 2019-12-08 23:52:57 -0600

---

<https://www.youtube.com/watch?v=UesxWuZMZqI>





a message which is sent between software components so it's the way you think about it said higher level than an IP packet



It's like emails it's not sending people it's in between software components.



the messaging deals with here's an example of a message and it represents a booking for a hotel system and the developers decided to represent the messages in the Json format





message payload typically also can include key value pairs.

For the message attribute,

You can put the things you want to filter on, route the message on.



you can entcryped the message palyload and the attribute remain unencryped . You can have quite deep nested structures but your message attribute are pretty well just named value paris





Queue are durable buffers for your messages and they have a pretty simple of operations is it will you can obviously send a message to Queue. You and once you get a confirmation that the message was stored and the message is supposed to be durable and won't go away.





On the other hand say you have massive consumers would pull the message from the queue and then I acknowledge that they are done which actually will remove message from the queue. Queue like a buffer between producers and consumers.



~~When you think about order to message processing and the question is can I actually use multiple workers to work on that perfectly order message.~~



~~The answer is if it's a singles stream order message: No . it has to be sigle worker processing.~~



so to allow you to build scalable message processors with FIFO.



We've introduced the concept of message groups and what we say is messages belonging to the same message group will be consumed in order



By introducing multiple message groups you can actually scale your feet of consumers



One example of where you might want to do that is if you're tracking website with a huge number of users interacting with the website so you would just use the session ID as a message group and that way each individual users session transactions would be processed properly in order, but you can still handle a huge number in parallel interleaved with no problem





Second concert in messaging is publish subscribe messaging using topics: send a separate copy to each subscriber so each subscriber to a topic will process all every single message that was published with that topic





The third concept in messaging is message stream. message stream are ordered and sending message to a stream to get suspended to the end of the stream



the typically also allow you to use multiple streams called stream charts or partitions depending a steam product



What is the difference between a stream and a Q and the main differences will when you send a message to a stream it is persistent and when you read it back you actually open like a cursor or an iterator on the stream and you can use them decide how and when to move the director forward and backwards



In the Stream when you don't do not delete the messages they are persistent for a pre-configured a Time



When and when you would use another and you should use queue when you can process your messages independently and when you're done messages deleted



You should you stream if you want to analyze the sequence of items in the Stream for analysis for incremental and differences between the items or if you want to run multiple analysis on the same stream using multiple processors







2.  how use queue to help your architecture

you have website, customer contact via the internet and calls go through the load balance.the calls go to feet of web service handling your traffic.



When someone make the booking for the hotel, there are second layer microservice deal with the booking and it sits behind another load balance. the booking system himself use the relation database as data store.



Most of traffic of the website is browsing the through the offers?. There are lot of traffic through the booking service and someone actually make a transaction in book a room



So what's can go wrong in the situation is

you a lunch a huge compaign and promotions and the customers are booking the hotel at the rate that you not prepared to handle.



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
