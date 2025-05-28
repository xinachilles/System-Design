# Storage

Created: 2017-10-08 17:06:06 -0600

Modified: 2020-11-27 16:34:41 -0600

---

![Storage --- Schema Message Table user_id+timestamp message_id int64 thread id int64 user id int64 content created at user id thread id participant_lds participant_hash created at updated_at timestamp Thread Table int64 int64 tekt string timestamp timestamp create _ user id+timestamp j son avoid duplicate threads index---true ](../../media/Message-What's-up-Storage-image1.png){width="5.0in" height="2.6875in"}![Answered Mar 2, 2012 I would imagine it would be something like: 'tableName, 'messages' , 'data' (This is generic as I am sure FB's schema is much more advanced) messages:author timestamp=1229953071910, value=The Author messages:body timestamp=1229953072029, value=This is the message messages:subject timestamp=1229953071791, value=He110 World data:time timestamp-1229953071791, value=20120301 data:generic timestamp=1229953071791, value=generic value Here is an example of creating schema for a blog: ](../../media/Message-What's-up-Storage-image2.png){width="5.0in" height="2.4375in"}

| row Key            |     |           | Message   |          |      |
|--------------------|-----|-----------|-----------|-----------|-------|
| message id         |     | sender id | thread id | create at | data  |
| Timestamp +user id |    | 2222      | 11        | 12:12:12  | hello |
|                   |    |          |          |          |      |





for Message table, why chose NO SQL



Messaging is going to be very write heavy.



For a write heavy system with a lot of data,



traditional database usually don`t perform well. Every write is not just an append to a table but also an update to multiple index which might require locking and hence might affect with reads and other writes.



However, there are NoSQL DBs like HBase where writes are cheaper. NoSQL seems to win here.



for send message, we don`t need any join operation



**Store Message inHBasedatabase**























API:





Send Message

sendMessage(senderId, recepientId, messageContent, clientMessageId)





**How does the messenger maintain the sequencing of the messages?**We can store a timestamp with each message, which would be the time when the message is received at the server.



Other solution is ,we need to keep a sequence number with every message for each client ( or for each thread?). This sequence number will determine the exact ordering of messages for EACH user. With this solution, both clients will see a different view of message sequence, but this view will be consistent for them on all devices.










