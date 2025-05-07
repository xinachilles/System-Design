# Scenario

Created: 2017-10-09 10:27:13 -0600

Modified: 2017-10-19 15:40:17 -0600

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

Other than the chat messages, we would also need to store users' information, messages' metadata (ID, Timestamp, etc.). Also, above calculations didn't keep data


