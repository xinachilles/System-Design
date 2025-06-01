# Summary 



---

**Function:**



1.  post tweet
2.  like button so the user can make the tweet as favorite
3.  follower and unfollow other user
4.  create and display the newsfeed base on user and his follower`s timeline. display those tweet base on...



**QPS**



we assume we have 600M DAU and each user everyday has 60 read request such as read the post and 0.1 write request



read peek * 3



**about the storage:**



we assume each post has maximum 130 characters and each character has 2bytes and we have around 30 byte for meta data of each tweet like post id, user id ..





**CAP**

also i think the system need high available

the consistency is also import but if the user cannot see the tweet a little w

**Service**

at the high level, i think we can have 4 different for the different request with the loader balance for the traffic distribution



we have 4 service : one is user service for user login, tweet service for post the tweet and store the tweet. other is media and photo service and a friendship for user follow and unfollow





**Storage**

**on the backend,**



1.  we can use a sql database to store the user information, the schema is



2.  we also need a friend ship database to store, user id







we need database store the tweet, it is read heavy system we can use no sql database to stroe the da



and use file system to store photo and videos

we also need a table linked the post and media this post contains



we can store this information in a sql database









**At the high level, we can go through the function one by one, improve my design**



**for post the tweet**



since we have a cache for all last 3 days tweet, when use post a tweet, they need write it to the cache first, write successfully if it write to cache and database



**news feed generation**





the new feed should be generated from user`s post and his friend`s post

when this system get the request to generate the news feed.



a.  when we open the app, we should get the friends list and cache it in the client side

b.  in the client side, we also should have a last fresh time

c.  client will sent to request to cache first. if we get enough data, we can just return those data to client other wise we need query the database base on user id



we merge user`s post and friend`s post together and sort this post base on the timestamp then return to the client.





there are some problem about this design:



1. the new data will not show unit user can pull it from the service and some time the there will be no data and pull request will return a null response.



2.  if client read the tweet more than 3 days and from database and the tweetid id is shading base on the tweet id and time creation. it will be crazy slow





the alternate approach is



when user publish a post, service can fanout to his online frinds -- write the tweeit to newsfeed table.



newfeed is store in nosql database, key will be userid and column key will be timestamp + tweeit id and value will be the data



when people fanout to newfeed table, the user will get notification and will read the data from the newfeed table







if it is online user, we can create a notification service












