# News feed Summary 2



---

**Function:**

1. post tweet , the tweet should contains text, image and video

2. create and display the newsfeed based on user and his follower`s post or tweet. Display those tweets base on...

3. follower and unfollow other user

**QPS**

We assume we have 600M DAU and each user everyday has 60 read requests such as read the post and 0.1 write request

read peek * 3

read QPS = 600 M * 60 /86400

write QPS = 600 M * 0.1 /86400

**Storage:**

We assume each post has maximum 130 characters and each character has 2bytes and we have around 30 byte for meta data of each tweet like post id, user id ..

for message : 600 M * 0.1 * (130*2+30) = 18 G / day

**CAP**

also i think the system need high available

the consistency is also import but if the user cannot see the tweet a little w

**Service**

At the high level, i think we can have 4 different for the different request with the loader balance for the traffic distribution

we have 4 service : one is user service for user login, tweet service for post the tweet and store the tweet. other is media and photo service to store the photo and video and a friendship for user follow and unfollow

1. we can use a SQL database to store the user information, the schema is user id ( it should be unique and as the primary key), email address, password which should encrypted time stamp when create this account.

2. we also need a friendship database, we can store it in the sql database and two column: user and follower

3. we need database store the tweet, it is read heavy system we can use no sql database to store the data

To create the timeline for any given user, one of the most important requirements is sort the post by user and time of creation

Since we will have a primary index on tweet id, it will be quite quick to find latest tweet id.

We can use epoch time for this. Let's say our tweet id will have two parts; the first part will be representing epoch seconds and the second part will be an autoincrementing sequence. So to make a new tweet id, we can take the current epoch time and append an auto incrementing ID from our key generating DB.

We can figure out shard number from this tweet id ( tweet id % 200) and store the photo there.

What could be the size of our tweet id ? Let's say our epoch time starts today, how many bits we would need to store the number of seconds for next 50 years?

86400 sec/day * 365 (days a year) * 50 (years) => 1.6 billion seconds

We would need 31 bits to store this number. Since on the average, we are expecting 23 new photos per second; we can allocate 9 bits to store auto incremented sequence.

So every second we can store (2^9 => 512) new photos. We can reset our auto incrementing sequence every second.

Like document database,

4. use file system to store photo and videos

5. we also need a table linked the post and media this post contains we can store this information in a SQL database

**API**

First we want to define the system API, it can explitily

System APIs

it's always a good idea to define the system APIs. This would explicitly state what is expected from the system.

We can have SOAP or REST APIs to expose the functionality of our service. Following could be the definition of the API for getting the newsfeed:

Parameters:

api_dev_key (string): The API developer key of a registered account. This can be used to, among other things, throttle users based on their allocated quota.

~~user_id (number): The ID of the user for whom the system will generate the newsfeed.~~

Should put the user ID on the token

since_id (number): Optional; returns results with an ID greater than (that is, more recent than) the specified ID.

count (number): Optional; specifies the number of feed items to try and retrieve, up to a maximum of 200 per distinct request.

max_id (number): Optional; returns results with an ID less than (that is, older than) or equal to the specified ID.

exclude_replies(boolean): Optional; this parameter will prevent replies from appearing in the returned timeline.

Returns: (JSON) Returns a JSON object containing a list of feed items.

**At the high level, we can go through the function one by one, improve my design**

1.  write the tweet or post

When I want to post a tweet, I just call write API and write the tweet to the tweet table

2.  generate the time line

<!-- -->

A.  Pull model: when user want to generate the timeline, first get all his follower then read the post from the tweet table

**N db reads**

This will be pretty slow. The improvement is

Offline generation for user`s newsfeed: We can have dedicated servers that are continuously generating users' newsfeed and storing them in memory.

Whenever these servers need to generate the feed for a user, they would first query to see what was the last time the feed was generated for that user. Then, new feed data would be generated from that time onwards. We can store this data in a hash table, where the "key" would be UserID and "value" would be a STRUCT like this:

Struct {

LinkedHashMap<FeedItemID> feedItems;

DateTime lastGenerated;

}

B.  We cache all active user who just use tweeter recently. Cache is just the most recently post (since the post寄 is sort by timestamp) and total number of post for this user

<!-- -->

C.  Push model, when user post a tweet, they also use async job write an entry or record to newsfeed table (id, owner id, tweet id and time stamp). when user want to generate the timeline, they just need query the newsfeed table

Find out all tweet id then grab the tweet from the tweet table

D.  We can combine 1 and 2 together. We can move all the users with high followings to pull based model and only push data to those users who have a few hundred (or thousand) follows

when people fanout to newsfeed table, the user will get notification and will read the data from the newsfeed table if it is online user, we can create a notification service



![](../../media/Twitter-^M-Insgram-Twitter---News-Feed-News-feed-Summary-2-image1.png)

