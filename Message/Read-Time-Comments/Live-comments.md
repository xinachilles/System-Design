# Live comments 



---

When client 3 starts watching the red live video, all it does is it sends a simple subscription request using a simple HTTP request to our server.



The server will store the subscription in an in-memory subscriptions table.



Now the server knows that the client with connection id3 is watching the red live video. Why does in-memory work?



There are two reasons. The subscription table is completely local. It is only for the clients that are connected to this machine.



Secondly, the connections are strongly tied to the lifecycle of this machine. [If the machine dies, the connection is also lost]{.mark}, and therefore, you can actually store these subscriptions in-memory inside these frontend nodes. ~~We'll talk a little bit more about this later.~~





Callback function ?





able to get to [100k connections per frontend machine]{.mark}. Anyone remember the second largest live stream?



so 5,000 events can be published per second from a single dispatcher node to frontend machine





Why not just do this with Kafka?" If you do it with Kafka, then yes, you do get that, because the way you would do it with Kafka is that the likes backend would publish a like over to a live video topic that is defined in Kafka, and then each of these frontend machines would be consumers for all the library or topics that are currently live.



You already see a little bit of a problem here, which is that these frontend servers are now responsible for consuming every single live video topic, and each of them needs to consume all of them because you never know which connection is subscribed to which live video, and connected to this frontend server



~~What this gives you is guarantees. You cannot drop an event anywhere here in the stack, but you can drop an event when you send it to the client from the frontend server, but you can detect that. In fact, EventSource interface provides a built-in support for it. It has this concept of where you are. It's like a number that tells you where you are in the stream, and then if their things get dropped, the frontend server, the next time it connects it will tell you that it was at point X, and the frontend server can start consuming from the topic at point X.~~ What you give away here is speed, and also the fact that the frontend servers will stop scaling after a while, because each of them need to consume from these streams, and as you add frontend machines that doesn't help. Each frontend machine now needs to still consume all the events from all the Kafka topics.





Notice that when the frontend server sends the data, or has the persistent connection to the client, it is actually a fire and forget. The frontend server itself is not blocking on sending the data to the client. It just shoves it into the pipe and forgets about it, so there is no process, or no thread that is waiting to figure out whether the data actually went to the client. Therefore, no matter what these clients are doing, they might be dropping events, they might not be accepting it because something is wrong on the client side. The frontend server is not impacted by that. The frontend server's job is to just dispatch it over the connection and be done with it, again, because we are going for speed and not for [crosstalk 00:49:24]







Charles Humble: It's a slightly cheeky question in a way, but why did you not build a system on top of something like Kafka, which would give you those consistency guarantees? I know that Kafka is used quite extensively within LinkedIn.

[13:30](javascript:void(0);) Akhilesh Gupta: So there are a few reasons and I'll come to consistency guarantees in a bit. If we used Kafka, I think one of the first constraints that we would run into is that the number of Kafka topics that we would need would be equal to the number of live videos.

So if let's say, we are running a hundred live videos at the same time, then we would need to have that many Kafka topics and then each frontend server would need to consume from all of these topics, because we cannot be sure whether a particular connection, which is watching a particular live video is connected to frontend server one ,or frontend server two.

If consumption can't keep up, then adding more frontend servers doesn't help because even if you add more frontend servers, it still needs to consume from all of these Kafka topics at the same time.

Then, live video is what we call a broadcast topic because there are many subscribers for the same topic, but we also have what we call personal topics where there's one subscriber per topic.

So we do this for things like messaging, because you get your own messages, or your own typing indicators. To do that, we now need millions of topics in Kafka to support that because each member has their own messages topic, or has their own typing indicators topic and millions of topics in Kafka just doesn't work, at least the way Kafka's built today. So these are like the fundamental reasons that we couldn't use Kafka, mostly scale issues.



about cross data center publish, in which we are able to do regular HTTP post requests across data centers, to make sure that a published event reaches all of its subscribers across all data centers. With Kafka, we would need to use Kafka MirrorMaker, which slows things down considerably because now it's trying to mirror each event in each topic across all data centers and we want to be fast. So that's the other thing that holds us down.

So yes, Kafka has that benefit that we could achieve absolutely reliable delivery and I think we do use Kafka for situations in which we need absolute reliable delivery.

For example, we use it for making sure that a message sent by a member is persisted guaranteed for the receiver, but for things like this, where we want to be fast and time is what matters, it turns out that the system that we built was way more efficient at doing that.


