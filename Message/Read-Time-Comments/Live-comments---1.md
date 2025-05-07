# Live comments  -1

Created: 2020-12-16 17:27:27 -0600

Modified: 2021-01-26 19:24:12 -0600

---

<https://www.infoq.com/podcasts/linkedin-realtime-messaging-architecture/>



<https://www.infoq.com/presentations/linkedin-play-akka-distributed-systems/?useSponsorshipSuggestions=true>



1.  [Subscribe follow]{.mark}

API subscribe(topic, callback function)



Topic: presencestatus:Bob





The first is the connection, the persistent connection (Http long polling) that allows the backend systems to push events to connected clients.



So, that's what I mean by connection. Then there's the client subscriber, this is client, the device which initiates the connection with the server and [subscribes to the topics that it is interested in.]{.mark}



the client establishes the initial persistent collection using a simple HTTP long poll request and it's also called the event source protocol. Then the subscription requests are just regular HTTP requests and all client to server communication is through regular HTTP post requests.



Then we have the [frontend connection manager.]{.mark} This is the first system in the backend that holds the persistent connections with the clients. Each machine in the system needs to manage [hundreds of thousands of connections]{.mark}, receive and store subscriptions, and dispatch publish events to subscribe connections.





what we call the event source connection. Event source is the fundamental building block that we use to actually create this persistent connection and it's simply a long poll connection.



the server communicates with the client by publishing events over the same initially established persistent connection and therefore all server to client communication happens over that one channel.





Then we have the [backend dispatche]{.mark}r. This is the system that allows us to scale, by dispatching between multiple front end connection manager machines for each published event. So these four building blocks are the fundamental building blocks that comprise the system.







Client sends a connection request to the [front end connection manager]{.mark} and we use what we call Akka actors to manage the lifecycle of each connection held by the frontend connection manager, so that's the connection request.



Then we have the subscription flow. The client initiates a subscription to topics. On receiving the subscription request, [the frontend stores it in a simple in-memory mapping of topics to the connections that are subscribed to them.]{.mark} Topic -> client



Then it also forwards the subscription request to the dispatcher, which stores a mapping of topics to the frontend machines that are subscribed to them in a distributed key value store.



Topic -> frontend machine



So we're done with the connection and we're done with the subscription.





[The next flow is the publish flow.]{.mark}



The published flow starts when a viewer starts to actually like a live video, so different viewers are watching different live videos, and they're continuously liking them. All these requests are sent over regular HTTP requests to the likes backend, [which stores them]{.mark} and then dispatches them to the dispatcher.



[It does so with a regular HTTP request to any random dispatcher node]{.mark}, and they look up the subscriptions table to figure out which frontend nodes are subscribed to those likes and dispatch them to the subscribed frontend nodes. The likes have now reached the frontend nodes, They need to send it to the right client devices.

Each frontend node will look up its local subscriptions table, and this is done by the supervisor Akka Actor to figure out which Akka Actors to send these like objects to. They will dispatch the likes to the appropriate connections based on what they see in the subscriptions table.

It can also do comments, typing indicators, seen receipts, all of our instant messaging works on this platform, and even presence. Those green online indicators that you see on LinkedIn are all driven by this system in Real-Time. Everything is great. We're really happy, and then, LinkedIn adds another data center.



[Cross data center]{.mark}



The dispatcher in the first data center simply dispatches the likes to all of its peer dispatchers in all the other data centers,





[how many persistent connections a single machine can hold.]{.mark}



It turns out that we were able to have 100,000 (100k) connections on the same machine.



H[ow many events per second can be published to the dispatcher node?]{.mark}



The dispatcher node only has to publish an incoming event to a maximum of the number of frontend machines. It doesn't have to worry about all the connections that these frontend machines are in turn holding. It only cares about this green fan-out here, which is the number of frontend machines that this dispatcher can possibly publish an event to, but it doesn't have to worry about this red fan-out. That's the part that the frontend machines are handling, and they're doing that with in-memory subscriptions



5,000 events can be published per second to a single dispatcher node.

[So why was [SSE](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events) chosen over, say, WebSockets?]{.mark}

Akhilesh Gupta: I think there are two main reasons. One is the platform that we were building primarily needed us to [send data from the server to the client]{.mark}. That is the one where we wanted it to be a persistent connection. SSE is really good at that, that one-way communication from server to clients. [The use cases that we didn't have were where you require bidirectional communication]{.mark}, (for sse,

Yes, it's possible.

You can have more than 1 parallel HTTP connection open, so there's nothing stopping you.

)where for example, you are doing real time metrics analysis or something like that. In which case, the client also needs to be able to send data to the server in a continuous flow. So, given that requirement of server to client communication, SSE fit the bill really well for us.

The second reason was that WebSockets is not always supported across all firewalls and across all network connections, if you look around the world. Therefore, we wanted to have a connection that is just supported widely. So, that was the second reason.

I think the third reason, now that I come to think of it is that the LinkedIn traffic infrastructure is also built to support HTTP/2 and given that the requests from client to server can also be piped to the same pipe over HTTP/2 and therefore, it doesn't really matter whether the client to server requests are going over regular HTTP versus over WebSockets.





~~I think you call it presence in LinkedIn terms, which again I thought was intriguing. I think there's a [blog post](https://engineering.linkedin.com/blog/2018/01/now-you-see-me--now-you-dont--linkedins-real-time-presence-platf) on this as well, but I wondered if you could maybe give a bit of a rundown of that particular scenario and how that works?~~

we have this connection and this connection really represents that a LinkedIn iOS, Android or web client is open, that it has an online persistent connection to our frontend machine and we could use this. We could use this to know that a particular member is currently online.

If you think about it, it feels like, "Oh, we already know that the connection is there and therefore it should be so easy to just know that a particular person is online and therefore just show the green dot." It turns out it's not so because there are all sorts of jittery connections, people going under tunnels and stuff like that.

So when we initially built it based on that, it would just go red, green, red, green, red, green, and it would just not make sense. So we had to build this thing called jitter detection, where we would smooth out all this jitter noise. The way we did it is actually very simple. [We simply emitted heartbeats from this connection to the frontend connection manager. These heartbeats, as long as they're there, we would consider a person online.]{.mark} so they would be emitted for example, every 30 seconds. If this stopped coming in for a significant period of time, and this is what causes the smoothing, because if they stop coming in for like two seconds, [doesn't matter but if they stop coming in for more than a minute,]{.mark} then it starts to matter, and that's when you go offline. Then we process these heartbeats, again using Akka actors, where each Akka actor is now representing an online member. Then use was that as our source of truth for whether a person is online or not and display that to our end users.

That's one final thing I want to mention there, which is how do we distribute whether somebody went offline to people who are viewing their presence? Again, we use the real time platform. [Everybody who wants to look at the presence for a particular member subscribes to their broadcast topic for whether they're present or not.]{.mark} If we detect that that person has gone offline, we use the real time platform to publish that information to everybody who is watching out for that person's presence.












