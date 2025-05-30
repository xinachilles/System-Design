# Generate TimeLine



---

**Ranking and Timeline Generation**



To create the timeline for any given user, we need to fetch the latest, most popular and relevant photos of other people the user follows.

For simplicity, let's assume we need to fetch top 100 photos for a user's timeline. Our application server will first get a list of people the user follows and then fetches

metadata info of latest 100 photos from each user. In the final step, the server will submit all these photos to our ranking algorithm which will determine the top 100 photos (based onrecency, likeness, etc.) to be returned to the user. A possible problem with this approach would be higher latency, as we have to query multiple tables and perform sorting/merging/ranking on the results. To improve the efficiency, we can pre-generate the timeline and store it in a separate table.



Pre-generating the timeline: We can have dedicated servers that are continuously generating users' timelines and storing them in a 'UserTimeline' table. So whenever

any user needs the latest photos for their timeline, we will simply query this table and return the results to the user.

Whenever these servers need to generate the timeline of a user, they will first query theUserTimelinetable to see what was the last time the timeline was generated for

that user. Then, new timeline data will be generated from that time onwards (following the abovementioned steps).



What are the different approaches for sending timeline data to the users?



1. Pull: Clients can pull the timeline data from the server on a regular basis or manually whenever they need it. Possible problems with this approach are a) New data

might not be shown to the users until clients issue a pull request b) Most of the time pull requests will result in an empty response if there is no new data.



2. Push: Servers can push new data to the users as soon as it is available. To efficiently manage this, users have to maintain aLong Pollrequest with the server for

receiving the updates. A possible problem with this approach is when a user has a lot of follows or a celebrity user who has millions of followers; in this case, the server

has to push updates quite frequently.



3. Hybrid: We can adopt a hybrid approach. We can move all the users with high followings to pull based model and only push data to those users who have a few

hundred (or thousand) follows. Another approach could be that the server pushes updates to all the users not more than a certain frequency, letting users with a lot of

follows/updates to regularly pull data.

For a detailed discussion about timeline generation, take a look atDesigning Facebook's Newsfeed.





**11. Timeline Creation withShardedData**

To create the timeline for any given user, one of the most important requirements is to fetch latest photos from all people the user follows. For this, we need to have a

mechanism to sort photos on their time of creation. This can be done efficiently if we can make photo creation time part of thePhotoID. Since we will have a primary

index onPhotoID, it will be quite quick to find latestPhotoIDs.

We can use epoch time for this. Let's say ourPhotoIDwill have two parts; the first part will be representing epoch seconds and the second part will be anautoincrementing

sequence. So to make a newPhotoID, we can take the current epoch time and append an auto incrementing ID from our key generating DB. We can figure

out shard number from thisPhotoID(PhotoID% 200) and store the photo there.



What could be the size of ourPhotoID? Let's say our epoch time starts today, how many bits we would need to store the number of seconds for next 50 years?

86400 sec/day * 365 (days a year) * 50 (years) => 1.6 billion seconds

We would need 31 bits to store this number. Since on the average, we are expecting 23 new photos per second; we can allocate 9 bits to store auto incremented sequence.

So every second we can store (2^9 => 512) new photos. We can reset our auto incrementing sequence every second.

We will discuss more details about this technique under 'DataSharding' inDesigning Twitter.



12. Cache and Load balancing



To serve globally distributed users, our service needs a massive-scale photo delivery system. Our service should push its content closer to the user using a large number of

geographically distributed photo cache servers and use CDNs (for details seeCaching).

We can introduce a cache for metadata servers to cache hot database rows. We can useMemcacheto cache the data and Application servers before hitting database can

quickly check if the cache has desired rows. Least Recently Used (LRU) can be a reasonable cache eviction policy for our system. Under this policy, we discard the least

recently viewed row first.

How can we build more intelligent cache? If we go with 80-20 rule, i.e., 20% of daily read volume for photos is generating 80% of traffic which means that certain

photos are so popular that the majority of people reads them. This dictates we can try caching 20% of daily read volume of photos and metadata.
