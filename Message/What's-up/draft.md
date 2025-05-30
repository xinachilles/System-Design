# draft



---

**Push service (jiuzhang)**

How to know which server holds the connection to which user? We can introduce a software load balancer in front of our chat servers; that can map eachUserIDto a server to redirect the request.

How should the server process a 'deliver message' request? The server needs to do following things upon receiving a new message 1) Store the message in the database 2) Send the message to the receiver 3) Send an acknowledgment to the sender.

The chat server will first find the server (**Push service (jiuzhang)**

that holds the connection for the receiver and pass the message to that server to send it to the receiver. The chat server can then send the acknowledgment to the sender; we don't need to wait for storing the message in the database; this can happen in the background.

c. Managing user's status

1. Whenever a client starts the app, it can pull current status of all users in their friends' list.

2. Whenever a user sends a message to another user that has gone offline, we can send a failure to the sender and update the status on the client.

3. Whenever a user comes online, the server can always broadcast that status with a delay of few seconds to see if the user does not go offline immediately. ( not injiuzhang)

4.Client'scan pull the status from the server about those users that are being shown on the user's viewport. This should not be a frequent operation, ( as the server is broadcasting the online status of users and we can live with the stale offline status of users for a while) (not injiuzhang).

(jiuzhang, at same time, client also tell serve that it is on line)

5. Whenever the client starts a new chat with another user, we can pull the status at that time.



Detailed component design for Facebook messenger

Design Summary: Clients will open a connection to the chat server to send a message; the server will then pass it to the requested user. All the active users will keep a connection open with the server to receive messages. Whenever a new message arrives, the chat server will push it to the receiving user on the long pool request.

Messages can be stored inHBase, which supports quick small updates, and range based searches. The servers can broadcast the online status of a user to other relevant users. Clients can pull status updates for users who are visible in the client's viewport on a less frequent basis.

6. Data partitioning

Since we will be storing a lot of data (3.6PB for five years), we need to distribute it onto multiple database servers. What would be our partitioning scheme?

Partitioning based onUserID: Let's assume we partition based on the hash of theUserID, so that we can keep all messages of a user on the same database. If one DB

shard is 4TB, we will have "3.6PB/4TB ~= 900" shards for five years. For simplicity, let's assume we keep 1K shards. So we will find the shard number by

"hash(UserID) % 1000", and then store/retrieve the data from there. This partitioning scheme will also be very quick to fetch chat history for any user.

In the beginning, we can start with fewer database servers with multiple shards residing on one physical server. Since we can have multiple database instances on a server,

we can easily store multiple partitions on a single server. Our hash function needs to understand this logical partitioning scheme so that it can map multiple logical

partitions on one physical server.

Since we will store an infinite history of messages, we can start with a big number of logical partitions, which would be mapped to fewer physical servers, and as our storage demand increases, we can add more physical servers to distribute our logical partitions.

7. Cache

We can cache a few recent messages (say last 15) in a few recent conversations that are visible in user's viewport (say last 5). Since we decided to store all of the user's messages on one shard, cache for a user should completely reside on one machine too.

8. Load balancing

We will need a load balancer in front of our chat servers; that can map eachUserIDto a server that holds the connection for the user and then direct the request to that server. Similarly, we would need a load balancer for our cache servers.

9. Fault tolerance and Replication

**What will happen when a chat server fails?**Our chat servers are holding connections with the users. If a server goes down, should we devise a mechanism to transfer

those connections to some other server? It's extremely hard to failover TCP connections to other servers; an easier approach can be to have clients automatically

reconnect if the connection is lost.

Should we store multiple copies of user messages? We cannot have only one copy of user's data, because if the server holding the data crashes or is down permanently,

we don't have any mechanism to recover that data. For this either we have to store multiple copies of the data on different servers or use techniques like Reed-Solomon

encoding to distribute and replicate it.

**Group Chat**

We can have separate group-chat objects in our system that can be stored on the chat servers. A group-chat object is identified byGroupChatIDand will also maintain a list of people who are part of that chat. Our load balancer can direct each group chat message based onGroupChatIDand the server handling that group chat can iterate

through all the users of the chat to find the server handling the connection of each user to deliver the message.

In databases, we can store all the group chats in a separate table partitioned based onGroupChatID.

b. Push notifications

In our current designuser'scan only send messages to active users and if the receiving user is offline, we send a failure to the sending user. Push notifications will enable our system to send messages to offline users.

Push notifications only work for mobile clients. Each user can opt-in from their device to get notifications whenever there is a new message or event. Each mobile

manufacturer maintains a set of servers that handles pushing these notifications to the user's device.

To have push notifications in our system, we would need to set up a Notification server, which will take the messages for offline users and send them to mobile

manufacture's push notification server, which will then send them to the user's device.
