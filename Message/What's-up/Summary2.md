# Summary2

Created: 2018-01-08 10:02:16 -0600

Modified: 2021-05-28 15:27:01 -0600

---

![User Id I Group Id I first time join the User DB Redis Group DB Client websocket service Client User service Friendship service Chat service group Friendship DB Session DB User id ->Safety: { unread_msg: 10; Last_msg preview: happy_hour_at_10:.. Timestamp: 21324 Chat DB History DB User lastime online Receiver ID I Send ID/ group ID I time stamp Message id I Encrypted Message I timestamp Receiver id I Sender id I message id I timestamp Message queue Chat service Online status 2 User A send a message B B is online A- > chat service -> chat DB -> historyDB-> online statu queue Service B is offline A -> chat service - >chat DB -> historyDB User B - > Friendship service -> Fried DB -> session DB ( for last stamp -> History DB for all unread message - > Chat DB ](../../media/Message-What's-up-Summary2-image1.png){width="5.0in" height="2.298611111111111in"}







**[Functional Requirements:]{.mark}**

1. Messenger should support one-on-one conversations between users.

2. Messenger should keep track of online/offline statuses of its users.

3. Messenger should support persistent storage of chat history( option)

4. Group Chats: should support group chat, multiple people talking to each other in a group.



Do we need to support picture/vedio

Do we need to support message status

Do we need to support user status/online/offline





[Non function requirement]{.mark}

Is Latency a very important metric for us?

Users should have real-time chat experience with minimum latency.

**3. Capacity Estimation and Constraints**

Let's assume that we have 500 million daily active users and on average each user sends 40 messages daily; this gives us 20 billion messages per day.

40 * 500 = 20 billion

QPS:

500 M *40 / 86000 = 200 k

(12 * 500 *40 = 240,000 )



[Peek]{.mark} QPS 200 k * 5 = 1M

**Storage Estimation:** Let's assume that on average a message is 100 bytes, so to store all the messages for one day we would need 2TB of storage.

20 billion (G) messages * 100 bytes => 2 TB/day

We assume we just need save five years of chat history, we would need 3.6 petabytes of storage.

2 TB * 365 days * 5 years ~= 3.6 PB

Other than the chat messages, we would also need to store users' information, messages' metadata (ID, Timestamp, etc.).

[How many websocket servers we need?]{.mark} Let's plan for 500 million connections at any time. Assuming a modern server can handle **50K** concurrent connections at any time, we would need 10K such servers

**High Level Design**

<https://docs.google.com/drawings/d/17bLFByJKflRHvkj08hX92DyfQ3rO1QFSEnFiGLJAd98/edit>





At a high level, we will need a master servers in the central piece for the communications between users.

1.  User or client login to the system through the login/user service


2.  Then client will ask the friend list, group he belong to



3. client need pull all unread message from system message table

For the one to one message, check the [message index table]{.mark}, pull all the message less then the client time stamp



we can store a timestamp in the client side means the last update time, when compare the timestamp with the table timestamp, if the change is new, than update the client information.

4.  user will connect to cluster of web socket service, and each web socket is for multiple user, then those web socket service will subscribe the pub/ sub system will push the message to user



When user log in, they need quest service a push service, the client use ~~a HTTP long polling~~ web socket to connect the push service. We need keep this connection open and as the server receives a message for that user. It can immediately pass the message to the intended user. ~~The long polling request can timeout or can receive a disconnect from the server, in that case, the client has to open a new request.~~





[User table]{.mark}

User id | Name | First time join



[Friend table]{.mark}

User ID | Friend ID | Create time



If we need to find out how many user in the group

[Group chat Table]{.mark}

Group chat ID| User ID | First time join|



[for Message table, dynamoDB]{.mark}



(Messaging is going to be very write heavy.

We can write the message to write-back cache then aysnc frush to DB)



[Message table]{.mark}

Message ID |message| timestamp

Message id is partition key

Timestamp is sorted key





[Message index table for checking the all history message]{.mark}

Sender user id could be the secondary local index



Receiver ID | Sender user ID | is send or receive | timestamp| message ID|



(may don't need this table)

Sender ID | Receiver ID | is send or receive | timestamp| message ID|





[Session table for unread message]{.mark}

.....Channle table

Receiver user ID | sender user ID | last view timestamp



[Group chat session table :]{.mark}

Receiver user ID | group ID |last view timestamp

(last view timestamp will be update when click the message)





[Channel Table(Redis): redis can handle 10^5]{.mark}



[(or just unread count table), user ID-> sender ID + unread count]{.mark} update this table after save the message the disk, update using lua script, need atomic operation



<table>
<colgroup>
<col style="width: 27%" />
<col style="width: 72%" />
</colgroup>
<thead>
<tr>
<th>key</th>
<th>value</th>
</tr>
</thead>
<tbody>
<tr>
<td>User_id: Scott</td>
<td><p>Safety: {</p>
<p>unread_msg: 10;</p>
<p>Last_msg_preview: happy_hour_at_10:..</p>
<p>Timestamp: 21324</p>
<p>}</p></td>
</tr>
<tr>
<td></td>
<td><p>David: {</p>
<p>unread_msg: 0;</p>
<p>Last_msg_preview: what’s up?</p>
<p>Timestamp: 341234;</p>
<p>}</p></td>
</tr>
<tr>
<td></td>
<td><p>Uncle: {</p>
<p>unread_msg: 0;</p>
<p>Last_msg_preview: dinner time?</p>
<p>Timestamp: 1234; // expired</p>
<p>}</p></td>
</tr>
</tbody>
</table>



**We will go through the function one by one to improve my design**

[**1.** **one on one conversion **]{.mark}

Server need maintain a hash table, where "key" would be the recipient userID and "value" would be the id of the websocket service



[or Chat server can calculate which websocket service belong to which user id by some hashfunction]{.mark}





when user A want to send a message to user B,

1.  user A will call [RPC call]{.mark} send a message to the Chat service,
2.  Chat service store the message in the database and forward this to dispatch service/ queue for user B
3.  A async job to update message [history DB]{.mark} for user B
4.  Chat service send a acknowledgment (ack) to client A tell him message send successfully
5.  the Dispatch services/ queue will forward that message to partial websocket service which connect the user B
6.  User B will have a web socket connection with websocket service
7.  Web socket service will push that message to client B
8.  B will send a ack to service tell service received the message



[client A also need a list to store the ACK waiting list to store the message already sent but not revived the acknowledge.]{.mark}

if in 3, client A did not received ack in certain time from service, client A need re-send the message ( 1. 3 is missing or 1 is fail )

if client A received ACK, we can remove the message from the queue.



[Client B also need queue or list to check the duplicate message since A many send a message more than once]{.mark}





**c. Managing user's status**

1.  Whenever a client starts the app, it can send the current status to push service and push service will forward to its online status service
2.  Online service will update the status on DB
3.  Then pass to online status dispatch service then pass it to his friends
4.  Client will send the a heartbeat to push service tell service i am still on line

<!-- -->

5.  Whenever a user sends a message to another user that has gone offline, we can send a failure to the sender and update the status on the client.

4. Whenever a user comes online, the server can always broadcast that status with a delay of few second to his friend ( not in jiuzhang)

**Data partitioning**

7. Cache

Write back ~~through~~ cache for message DB

**Group Chat**



when user A want to send a group ,

1.  user A will first pass the message to the Chat service,
2.  Chat service store the message in the database and forward this to dispatch service/ queue for all the user in this group if the user is online
3.  Update message history DB for all users
4.  Chat service send a acknowledgment (ack) to client A tell him message send successfully
5.  the Dispatch services/ queue will forward that message to websocket service which connect the group users
6.  Online group users will have web sockets connection with websocket service
7.  Web socket service will push that message to all oneline users
8.  Oneline users will send a ack to service tell service received the message







~~We add a channel service between push service and web service. When we login and user belong to a group, User will subscribe a channel service and channel service will maintenance a group id and one user list map. Web message or master service will only send one message to channel service and change service will copy this message and send to all one line users in the group~~

web service also need store the group message in the database ( key will be the group id )



ACK, do we if channel service send the message to client, client send a ACK to service, the problem is if there is a huge group, service will receive too many ACK



the solution is client don`t need send the ACK for every message, [client will send ACK every minute and store the last ack time or last message id]{.mark}

we need maintenance a table call group user: group id, user id and last ack time or message id



**Message sequence:**



it is difficult to maintenance the message sequence, since network issue we cannot guarantee the message arrived at the service a the sequence



and client side don`t guarantee the same clock time



if A send message to B, we can guarantee the message at certain sequence

by at the sequence number in client side, for example A send message to B , we need add a sequence like message_1, message_2 ...



if client receive message3 first, then message1 and message2, client B should insert message 1 and message 2 in front of message 3



if it is group message, service will decide the message sequence, service will add a sequence number to message.



·



