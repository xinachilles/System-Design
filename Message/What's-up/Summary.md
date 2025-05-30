# Summary



---

Functional Requirements:

1. Messenger should support one-on-one conversations between users.

2. Messenger should keep track of online/offline statuses of its users.

3. Messenger should support persistent storage of chat history( option)

4.Group Chats: should support group chat, multiple people talking to each other in a group.



Push notifications: Messenger should be able to notify users of new messages when they are offline.



CAP requirement



How important is Consistency for us?

Our system should be highly consistent; users should be able to see the same chat history on all their devices.



Is Latency a very important metric for us?

Users should have real-time chat experience with minimum latency.



Consistency vs Availability?

Messenger's high availability is desirable; we can tolerate lower availability in the interest of consistency.





3. Capacity Estimation and Constraints

Let's assume that we have 500 million daily active users and on average each user sends 40 messages daily; this gives us 20 billion messages per day.



QPS:

500 M *40 / 86000 =200 k

Peek QPS200 k * 5 = 1M





Storage Estimation: Let's assume that on average a message is 100 bytes, so to store all the messages for one day we would need 2TB of storage.

1000 * 2,g000,m000,k000

20 billion messages * 100 bytes => 2 TB/day

Although Facebook Messenger stores all previous chat history, but just for estimation to save five years of chat history, we would need 3.6 petabytes of storage.

2 TB * 365 days * 5 years ~= 3.6 PB

Other than the chat messages, we would also need to store users' information, messages' metadata (ID, Timestamp, etc.). Also, above calculations didn't keep dat



**High Level Design**

At a high level, we will need a master servers in the central piece for the communications between users.

user, client login to the system through the user service.

we load the friendship list from the friendship service

system will find out which message is unread for that user

When a user wants to send a message to they will connect to the chat server and send the message to the server; the server then passes that message to the other user and also stores it in the database.



master can also control the status of user : user online or offline

**on the backend**

we need a thread table: the schema is

for the participant hash is for the group chat, we want to make sure same people cannot create another thread it



for Message table, why chose NO SQL:

Messaging is going to be very write heavy.



For a write heavy system with a lot of data, traditional database usually don`t perform well. Every write is not just an append to a table but also an update to multiple index which might require locking and hence might affect with reads and other writes.



However, there are NoSQL DBs like HBase where writes are cheaper. NoSQL seems to win here.



for send message, we don`t need any join operation













we will go through the function one by one to improve my design



(if we choose the pull mode, user can periodically asks service about the new message



then the server needs to keep track of messages that are still waiting to be delivered, and as soon as the receiving user connects to the server to ask for any new message, the server can return all the pending messages. To minimize latency for the user, they have to check the server quite frequently, and most of the time they will be getting an empty response if there are no pending message. This will waste a lot of resources and does not look like an efficient solution.)





1.  one on one conversion

when user log in, they need quest service a push service

then client user a a HTTP long polling connect that push service . we need keep this connection open and as the server receives a message for that user. it can immediately pass the message to the intended user. The long polling request can timeout or can receive a disconnect from the server, in that case, the client has to open a new request.

server also need maintain a hash table, where "key" would be theUserIDand "value" would be the connection object. So whenever the server receives a message for a user, it looks up that user in the hash table to find the connection object and sends the message on the open request.



if the receiver disconnected, we can keep the message on the service side for a while and wait the user reconnect.

we store a last logoff time in the user table database, we user login again

Users can ask the server if there are any new messages for him which create time greater than the last login time



How many chat servers we need? Let's plan for 500 million connections at any time. Assuming a modern server can handle**50K**concurrent connections at any time, we would need 10K such serve

the chat servers that can map eachUserIDto a server to redirect the request.

when user A want to send a message to user B, user A will first pass the message to the chat service, chat service store the message in the database,

The chat server also find the push server that holds the connection for the receiver and pass the message to that server. 2. The chat server can then send the acknowledgment to the sender;



c. Managing user's status

1. Whenever a client starts the app, it can pull current status of all users in their friends' list.

2.  client will send the a heartbeat to service tell service i am still on line and ask his friend`s status.

3. Whenever a user sends a message to another user that has gone offline, we can send a failure to the sender and update the status on the client.

4. Whenever a user comes online, the server can always broadcast that status with a delay of few second to his friend ( not injiuzhang)









Data partitioning



Partitioning based onUserID: Let's assume we partition based on the hash of theUserID, so that we can keep all messages of a user on the same database. If one DB

shard is 4TB, we will have "3.6PB/4TB ~= 900" shards for five years. For simplicity, let's assume we keep 1K shards. So we will find the shard number by

"hash(UserID) % 1000", and then store/retrieve the data from there. This partitioning scheme will also be very quick to fetch chat history for any user.

In the beginning, we can start with fewer database servers with multiple shards residing on one physical server. Since we can have multiple database instances on a server,

we can easily store multiple partitions on a single server. Our hash function needs to understand this logical partitioning scheme so that it can map multiple logical



7. Cache

We can cache a few recent messages (say last 15) in a few recent conversations that are visible in user's viewport (say last 5). Since we decided to store all of the user's messages on one shard, cache for a user should completely reside on one machine too.



**Group Chat**

we add a channel service between push service and web service. when we login and user belong to a group. user will subscribe a channel service and

channel service will matainance a group id and one user list map.

web message will only send one message to channel service and change service will copy this message and send to all one line user in the group



web service also need store the group message in the database ( key will be the group id )









- 










