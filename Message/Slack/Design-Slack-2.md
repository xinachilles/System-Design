# Design Slack 



---

for QPS, if we have 1000 message/s we need more than 2 machine



if more than 1000 qps, you need multiple partition



one my sql services can handle 500 G



cache will reduce 10-100 times

![Data MOdel User Table (SQL): Id (shard key) uuid name Friend table (SQL): Shard_key: user_id_l user id 1 david id user id 1 Scott id user id 2 Scott id user id 2 david id Group membership Table (SQL): group_id uuid uuid User_id(shard key) david id Scott status Online connected date 1234355 connected date 1234355 join_date 1232r54 1234355 timezone Los Angeles Last view date 12345354 Last view date 34121455 role user Admin team safety Last view date Last sec. 124153456 ](../../media/Message-Slack-Design-Slack-image1.png)



![Channel Table(Redis): key User id: Scott Message Storage Table(DynamoDB): value Safety: { unread_msg: 10; Last_msg_preview: happy_hour_at_l Timestamp: 21324 David: { unread_msg: O; Last_msg_preview. what's up? Timestamp: 341234; Uncle: { unread_msg: O; Last_msg_preview. dinner time? Timestamp: 1234; // expired PartitionKey containerld Sort key timestamp - msg_id Msg id uuid Sender id message System design is fun. Creation timestamp 413412 In group container id is group ID In direct message, container id is sorted {useridl, userid2) ](../../media/Message-Slack-Design-Slack-image2.png)







we need a channel table for one-on-one chat or group chat



channel id, ....



we need another table map the channel id ->user id

many to many

shard by user id





the message table the partition key can be channel id



we store who is oneline one the channel service or notification service



channel table is temp table just stored in the messory







![1 ](../../media/Message-Slack-Design-Slack-image3.png)



