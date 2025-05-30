# New feeds



---

Hi Clement so today I want you to design the Facebook news feed



Face book new feeds seems like a pretty larger features, the Facebook news feed her that has a lot of sub features or smaller components do it ,like for instance you can obviously load and view a user's news feed, you can interact with the user is Newsfeed by liking ,status updates for posting, status updates and also refresh a Newsfeed with new posts, like in real time



so I guess my question is what part of the Facebook Newsfeed are we are designing exactly



so just focus on the core functionality of the feed, so posting a status, get in your newsfeed ,an update on your newsfeed with new posts



don't worry too much about what goes on what goes into a post or the API for that for that matter, I just want you to do feed creation and refreshing, so how and when it gets constructed and updated with new post from your friends



okay by post can be pretty complicated, right they can have pictures or videos, special types of status updates, are we are we handling that ?do not care about my friends since like how we would store no bunch of videos or that kind of stuff





that's right we don't really care about this for the purpose of this interview, we can threat post as opaque candidate and it is that will certainly want to store ,but without worrying about the details of storage, the ramifications for large files like images and videos and so on



I guess what I'll write down here, really caring about loading a Newsfeed ,loading and updating the newsfeed right she said updating with new post



so loading an updating and I'll write it and ask for news feed and we care about posting, but this is more about like posting as it relates to updating a news feed is that correct



yes that's correct, okay I'll just put kind of like an arrow ,here with his we really care about the ramifications of posting on a user's Newsfeed ,but then are we are we want to deal with the system or the service that actually generates news feeds , because as far as I know it's quite complicated to create a Newsfeed ,you want to you want to actually like a rank the the most relevant post



do we care about that



or so no you're not designing to the ranking system or the ranking algorithm you can just assume you have access to that and you can simply feed it a list of relevant post and it generates the new feed for you to display



that simplifies our system a little bit ,or at least allows us to focus on other areas of the system ,are we concerned with showing ads to users? cuz I guess ads is also similar to ranking where you probably need some sort of entirely different service handling that and figuring out what ads are most important for user



so that's something that we don't want to worry about either? so think of that as a maybe like a bonus part of the design ?if you can find a way to fit it in or figure out where might fit then that's great but don't focus on that at the start at least





no ranking organize, it understood that it's just taken for granted ,no ranking and no ads



as far as scale ,I know the Facebook is a huge platform, but how big of an audience are we serving here, and is audience is our audience Global or



so obviously we have a global audience like Facebook does, in the system and let's kind of round things round the numbers out so we have 1 billion users sorry with a hundred ten million new status updates posted every day



1 billion users per day on the platform 10 million posts per day



until I guess is it fair to assume that you know we would have something like ten billion news feeds loaded per day on average loads it 10 times or maybe 5 billion per day



10 million post per day per day here and we said this is like 5 billion news feed loads per day



okay so we're really dealing with gigantic scale here one other number that I think would be really relevant would be the number of friends, that every user has at least on average because I'm thinking it looks like one of the major parts of the system that we're designing is the effect of posting a status update and having that post update the news feeds of other users and and who are those other users their the friends of the person posting



so how many friends can we assume on average a single user has let's go with a nice number like 500 for the person at the end of the interview but as you can imagine right some users are going to have way more friends some more going to have way fewer on what's 500 as it happened is





it fair to say that like you would be kind of a bell-shaped distribution, for as far as like friends goes, like some people have like a lot of friends, but some have none but on average is the majority of people have 500



let's say that I guess the average is really what's going to be the most important, but I'll just draw Vista to indicated this might be bell-shaped ,he leave those few people who have a lot of friends might actually be problematic,so we might have to handle them in the system will see a little bit later



I guess the next question would be when we do Post in you status update and we want to update news feed ? how fast do we want this update to happen, and what really worries me is the fact that we're dealing with a global audience, so we might have people who are our friends who would have to see the new post but who might be in a halfway across the world from us, are we okay with maybe having in a location-based discrepancies in terms of latency for news feeds to reflect a new post



so for instance if you post something then one of your friends who are close to you soon the same within the same region should see the post within a few seconds, but I think we're okay if you know a friend you have like in Europe for example, sees it was much later, with say with enough a minute



okay so I guess I'll put you know latency seen you post is going to be somewhere between 1 seconds to 1 minutes, depending on how far away people are, this is going to be I get for our ballpark estimate is that is that accurate



okay and I guess the last question that I think I have would be regarding availability, what kind of availability are we aiming for the system



so your design should shouldn't be relying on every machine being always up, how you shouldn't it shouldn't go completely unavailable or fail from a single machine failure ,but it's not a high availability requirements a but however you're the posts shouldn't ever just disappear, so once the user client gets a confirmation that the post was created. You can't lose that post



basically we want like post has to be maybe I'll write this in a red ,post have to be persistent, and telling me, posts would be persistent, but we don't necessarily care about high availability would still want good availability as as you can imagine for such a system ,okay so I'll just put down the persistence and no no-hire availability



Okay so I think I have a most of the information that I need to start tackling this design, [I feel like this design is really going to be divided into two major subsections]{.mark} ,[is the entire flow that starts at a user posting a status update ,and then there's the entire flow that starts that a user loading their Newsfeed,]{.mark} for the first time during the day for example or for that matter but then these two core user actions are very much related, if I get myself some space here I'll go to the very bottom of my cannabis you can imagine that we have two actions, we have created post, so create post and he said we don't really care about the API, like message signatures for these API here



so I won't go into that then this is good to be API call that a useror a client would call if they're posting a status update. create post, then you also have somewhere here in the other the other main function in which is get news feed



these are the two the two core user actions, and then where do we go from here, but like I said before there's going to be some kind of bridge between these two things, because we said that we want a seed like here let's say we have a feed we want this feed it to get updated with a new post



so there's going to be some sort of link between these two to the parts of the system put this Arrow up here I'll get to it in a little bit like this will not stay there the point of this event these are related parts of the system or connected in some way



we're dealing with billions of users, we're dealing with a lot of users so well I'm not exactly sure how we're going to design this yet, what I do know is that whatever servers handle these API requests here like they get feed and create post or going to have to have some heavy load balancing, it was that kind of scale is it impossible to do without



I'm trying to figure out is like the kind of the kind of server selection strategy that we would have her load balancing, I feel like that might give me an idea of where we want to go with this ,because when you're creating a post right ,you don't need any kind of cashing or your server stickiness, just you're just creating a poster just adding data to the database, so I think that here start this part of the system off ,we can have like some load balancing here ,or you know cluster load balance and these could just reroute our create post request to some API servers, that would handle this API calls in a round-robin type of fashion, because we like I just said we don't need any type of cashing or service stickiness



that does that kind of makes sense for this part of the system, okay so it's alright load balancing whoops load balancing with a round-robin ,are you going to be around Robin and the idea is this goes to a bunch of API servers try to figure out how to use API servers actually you know handle these requests in a second



but what happens here with a get feed? here we also need load balancing I don't think that for feeds though we want round-robin ,because if you're getting a seizure getting data that is stored somewhere, and I feel like the it's a user a single user might request their feed multiple times a day ,



so let me table this for a second, and try to figure out where our actually storage solution leave because I feel like that might be that might be indicative as far as like what would happen here



so when we create a post, we want to store our post somewhere, and he write things that having some form of relational database that stores and all of our posts ,maybe all of our users everything would make sense ,but actually going to write in blues here,we'll have like a relational database, I'll put RDB, main database as a question is clearly these API servers are eventually going to have to store a post in a relational database, maybe I'll put like a tentative Arrow here but just for now like I'm not sure this is exactly going to go directly to the database but idea they will eventually have to store for the Post



another question is would these feeds or what these clients is doing these get feed calls, would they go directly to the database to get the feeds?



clearly generating feeds is going to have to happen from the database at one point or another, but it feel like it would be a lot a lot of query the database if you were generating the seeds, you notes from this API calls directly here



also like we said like it would make sense to have this be it would make sense some load balance happened here and I feel like you would want for something where you're getting news feeds that are relevant to particular users



you would want or like we want to charge your database based on the user's or like based on the user ID for instance since but I'm kind of scared of starting this database here because we said that we would store our posts in here and maybe even our users and everything and if we were discarded this database then to generate a Newsfeed we would have liked perform queries across all of our shard this seems I like a nightmare



but what I'm thinking is that we may want when they want to treat a Facebook news feed as a separate piece of data, in other words we have our main relational database here that's for his posts at the very least and probably users as well ,[but news feed would be stored separatel]{.mark}y, we could score news feeds in such a way that we shared whatever storag solution a has the feeds, that's sort of what I'm thinking





and actually this might be good because we mentioned before that we're going to need some kind of ranking algorithm which who said we might have for free out of the box but that ranking algorithm is likely going to be the same that actually said generate news feeds from this database





okay so I know I've been I've been saying a lot, let me see if this makes sense, the idea is this is our database where we have in posts and users stored, here we have a bunch of shard that store news feeds for particular user, so users are going to get like these API calls are going to get forwarded to relevant shards that store the new feeds for the users, I guess I'll right here we have a load balancer, that is bouncing with hashing the user IDs for example, let's say that we track we track user IDs so use i d hashing,



and here we then have a bunch of shards ,which alright I guess I can read them in Blue, so that they are storing news feeds, so we'll have them like one two, and we have a lot ,we can probably estimate how many we have in a second I'll put some Dots here to show that we have a lot but the idea is this load balancer then goes to the various shard, these API calls go to the load balancer of here



[we can have the ranking algorithm that lives in between these and shards that hold the news feeds and this relational database,]{.mark} and the ranking algorithm is going to be the same or the ranking algorithms service is going to be the thing that actually [fetches a bunch of post from the database, Aggregates them generates a Newsfeed by applying the algorithm]{.mark} and then sends the new speed to these shards on a recurring basis, you know maybe like every 5 minutes every 10 minutes every hour depending on how fast we want news feed to be updated, throughout the system or for the product



Does this strategy make sense until I continue draw



for another reason that you didn't mention which is the fact that the feed will likely get paginated, so you're not going to return the entire feed to the user, when they try to get the feed, rather you'll send me the first 20 posts or items in the feed and then they might they might request a 20 ,the next 20 items after a while ,and so by store on the news feeds and having the starting strategy you'll be able to just serve it in, for example if your ask for the second page , the query for the second page will go to the same machine as a query for the first page. So you'll just be able to serve what you already kind of computed



so we given might have actually hundreds or thousands of post, we just looks like it's a single page on Facebook, but it's actually multiple Pages behind the scenes



This get feed API call basically is like this is a paginated API call ,we we make a request that goes let's say it's like this Shard here ,and this shard us returns maybe like the first page we could be the first 10 or post and then if we if we were past the second page, we get the word it again to this shard that already has the second page is that what you're what you're saying



~~Okay cool so this makes sense then I think I'm going to go with this idea then when he just Erase here these rectangles that overlap~~ and I guess here we can write and [we can have our ranking service this would periodically go to this database and periodically go to all the shards these are all seeds shards]{.mark}



they're all kind of like equivalent to each other they just happen to store news feeds for different users or different groups of users ,and here do you want me to go into more detail as to how this ranking service works or can I just assumed that like works out of the box here



just assumed this works it's likely to be a cluster of machines with probably some really smart cashing logic inside but for the sake of the question let's just assume it's it's kind of a black box that gives you what you want



as earlier you said it was a bonus, I think ads would probably live somewhere here to ,here we might have ads service and this would also somehow talk to the feeds and inject as in them, but let's not dive into it that's okay right I can just skip this for now



yeah that's perfect okay cool I'll just put an arrow to set of this would talk to the feeds likely and then some database maybe there's a separate database for ads



as far as how many of these feed shards we have ,we probably need to estimate that because maybe this is does fall apart here so we said that we have how many users do we say we have a billion users per day ,10 million post per day, I guess leave the 10 million post isn't really relevant here we're really just talking about the number of the news feeds, that we would have to store in these in these feed sahrd



the fact that are 5 billion use feed loads per day that's still find ,we probably is storing all the different variations of the newsfeed here, or probably these feeds cards get updated periodically like we said from the ranking service so what really matters is I can we have a billion users that are going to be storing a billion new feeds in those shards



are that are that are that have a billion new feeds stored in these shards, so let's see if a news feed has 1000 post, if we're talking about that pagination and how seemingly scroll Forever on Facebook and never run out of post ,I think a thousand post per Newsfeed is a good estimate you you agree here



thousand post should be should be enough and if they run out we can just get some more from the database as long as it doesn't happen too often, I think it's okay



okay so let's go with us as our estimation and then single post is an upper bound ,maybe like 10 kb, want to do it because we can store images and videos and posts, I know that you said that way we don't have to worry about how we're going to handle that but the point is a post can have a lot of data in it, so I'm just going to give ourselves an upper bound of 10 kilobytes per post, if we have a thousand post of 10 kb in every news feed in a billion Newsfeed, that's a thousand times 10 kilobytes that's 10 megabytes per Newsfeed





so maybe write this here that's 10MB per user news feed I think you meant 10 kb * 1000 post



so yes 10 megabytes, then we have a billion users ,



do you have 10 petabytes of data, I think we can have all of these machines , all of those shards ,so we have 10 terabytes of storage and I guess we would use disk here we couldn't store this in memory ,so we would we would store or news feeds in a thousand shards or a thousand machine carrying 10tb each and then use these would be stored on disk rather than a memory ,which I think would be like when we're requesting a news feed the difference in latency, between that saying something from memory reading it from memory verses reading it from disc ,doesn't seem like it's the thing that's going to make or break our product for loading news feed ,does that okay does that seem sensible



this whole thing here is a total of 1000 machines or a thousand shards that has 10 TB of storage each



and I guess the more I think about it we said they were dealing with a global audience right, and we did say the beginning we haven't yet tackled what happens when someone creates a post, and we're supposed to update our news feed ,will get into that in a second but we did say that we were okay with having some latency difference between local users versus users across the world that what that's correct right



then is that this entire system that I'm drying out here, this might be like a per region type of system this might be like you know a subsystem within a greater system this is just for one region, and then we might have this exact same system in another region, and these two systems would have talked to each other to the asynchronously update each other ,when they're new posts or things like that and if we you go with that kind of approach, then this means that this is kind of an upper bound like we actually might not need a thousand Machines of 10 terabytes each, because we might only be serving you know if we have 10 reasons we might only be serving one tenth of a billion users in this part of the system, does that make sense of what I'm getting at



what I'm going to do is I'm just going to say that this is up bound for about a thousand stars of 10 terabyte each, is an upper bound, if we want to support the entire Facebook population in this part of the system ,but it is an upper bound because we're going to have different region and I think the different region will come into play a little bit later once we get into the updates that happen when you create a post



[that is the next step now is what happens when you when you create a post and you need to update these feeds,]{.mark} because I guess thinking of the Facebook news feed when you're scrolling down the Facebook news feed, is the idea that if a user is scrolling down and one of their friends post a status update, they should suddenly see that status update appear may be like in their next page of a news feed posts, that how it works





when a new post comes in to the relative user, the next fetching of their feet or the next page might contain



it cuz it could I get the other option would have been, if your newsfeed like notifies the user that hey there's a new post, but that would be kind of similar ,if that were the case, we probably would have some kind of like feeds shard push to clients but you're saying that we don't have to worry about that for now



we can just assume that were that we're going to be getting you post in the next page of a news feed



~~the logic would probably be somewhere there would be a piece of logic inside the feed shards that wouldn't be super complicated because they need to get updated when there's a new post anyway ,so it's just about come out routing that back to the use or somehow~~



so the new post will get inject into the next page of the news feed the client request and I guess then just to make it clear here when we have our client is suing get feed called it's a really make it clear sight "get feed page 1"



then the shard return the first page of the feet ,and it tells the client was the next pages, so that when the client is kind of scrolling down it is who's another request to get paid number 2, a number 3 in



so now we go back to this idea of what happens when we create a post and we need to update our feeds, following the feature that we just described, well we said that our API server is here and I should write that these are API server clearly have to store the post in a relational database and then this post is going to be grabbed by the ranking service the next time that it generates news feed,



but we said that we need post to appear in a feed in the order of seconds ,right anywhere between a second and minutes depending on the region of the user, so if our ranking service Aggregates posts and generates new feeds every 5-10, 60 minutes, that's not going to be enough so we're going to need a second thing like these API server is need to need to somehow apart from writing the post in the database, we need to somehow notify the feeds shard that there's a new post and that they need to grab that you post an added to their Newsfeed



so I think that this is where this bridge might come into play, so let me actually erase yeah and I'm thinking that it might make sense to use a pub sub system here ,because we need we need all of the posts that are created here to appear, to appear these feeds, but we have a thousand different feeds, by way of a thousand different feeds, that need to be notified essentially in real time of a new post, so having a multiple like Pub sub topics up here act as the bridge between these two parts of the system ,and having these feeds or these feed shards basically subscribe to the pub subtopics seems like a sensible strategy



and I guess the idea the pub sub topics, we would have a one-to-one mapping Between pub sub topics and feed shards there would be as many pub sub topics as there are feed shards, the pub sub topics would only carry post for particular users, we would use the same hash strategy that we use down, here I think that would Tire or two systems together very well does that make sense



okay so then I think we'll have Pub subtopics he will have one purpose of topic second pups of topic will have a bunch of them and these topics they're going to take in messages that that holds the post, that needs to be sent to a particular users feed, these seeds shards are going to subscribe to these topics ,to hear you know this feels hard kind of subscribes to the top topic maybe and ,as new posts come in, the new posts that they basically are fed into the shards



these API servers have to do the secondary action after they've store the post hear, of sending a post to a topic, but they have to send it this is where the hash comes into play, they have to send it based on a hash of the user ID, but these user IDs are the their friends of the person creating the post,



so we actually need to get all of a user's friends, gets their user IDs ,based on those user IDs, follow the post to the topics and that's how they get routed to the correct feeds ,



the first the create post API has to go to the relational database because is probably the relational database store user IDs or use a rows, and every user we can probably assume has a column of its friends or the user IDs of all its friends and from there we grab those user IDs and then we do the hash, does that would be the ideal does that make sense



the only problem here is what if we had like this is where we said we have 500 friends on average per user, that seems okay to send you a 500 pubsub messages ,but what if we have those users that have a lot more friends like power users so to speak, maybe we don't need like past a certain number of friends, we don't need to have all of those friends receive updates on their Newsfeed immediately,and I'm assuming that may be friends in the database or rather users in the database might have some kind of last online column or maybe we have some other way of knowing when a user was last online and we can basically you notify only the most active users of the most recently active users that they have a new post that should be thrown into their feed, those that kind of makes sense to limit the number of notifications that we need to send that onec





yeah we definitely don't want to be generating to too much traffic if someone has like ten thousand friends ,we don't want to be sending too much traffic over the internal Network



find because many people who just aren't online on Facebook everyday, so if we have some way of filtering those people out ,and also I supposed to hear we said that we're dealing like this entire system that I'm designing here is only for one region like I mentioned you will probably have a duplicate version of this system for another region so we can actually also filter by user region here and only send this new post like in this part of the system only to the users in that region or the friends in that region





once we have all this information from the database,we can basically route to the appropriate topic based on user ID hashing, and there is one problem that I am thinking about, is that user ID hashing



the one problem that I'm thinking about though is that these API servers here, or performing a lot of actions like critical actions, they need to score the post in a relational database, this is because we supposed the post to have to be persistent, but they also need to update the news feed that we run into this problem of what if something goes wrong here, and like maybe we don't even for the Post in the database, that's not good we don't want that to happen but what if we what if we do store the post but we don't notify people of their feeds ,that their feeds need to be updated,that's not super great



what if we do the opposite or we only update the feeds but we failed to score the post I feel like we need we need some kind of better system here, I don't think these API servers would be the ones handling all these actions and also I guess I'm forgetting one thing since this is a region system a regional based system, we said that we do need to notify people in other regions, it just so happens that we can have like no longer latency these API servers need to do another thing, they need to notify the other region that something needs to happen at a post just came in, and users need to be updated for like there's some third action here, that needs to happen that goes to another region we can talk about that in the second the point is API servers right now or doing too much, I think with not enough like security or not security safety rather



so I think we have to have some solution here I'm wondering if we can just go with a messaging system again, a lot Pub sub where know we might not need a bunch of different Topics in per user ID, or with for clusters of user ID based on hashes ,maybe we only need one Pub sub topic with multiple partitions are using Kafa or something, for the point is I think we need that that message queue to you that gives us that guaranteed at least once delivery idea is API servers once they get a create post call, they notify some Pub sub topic that says hey I just got a post ,you have to do three things now or rather than subscribers to that top, you have to do three things now get the store the post in the database, you have to follow the post to all of the friends here, following the logic that we said before ,and you have to notify the other region,and if we have these messages in a pub sub topic we had that at least once delivery guarantee does that make sense like you think having another Pub sub messaging system here is sensible



~~yeah I actually think it's it's really nice especially since you might have some Logic for detecting free sample box, trying to create posts, and you might have some free processing of the post itself before you want to store it in the database, and things like this don't necessarily need to happen on the API server so I think it makes sense. It happen asynchronous after as a consumer or something pops up topic~~



I might want to do post processing or or even like feeding posts into some data pipeline or no some machine learning model or algorithm so this makes a lot of sense ,so this feed up API servers to do what they do best with his just that to take a request handle it by you by sending it to the pups sub for instance and then just going back to handling request, they don't need to do other stuff



the idea here would then be we have our Pub sub system, I guess I'll just write like I'll go one Pub sub topic, here depending on what technology we're using, we might have multiple partitions in the pups sub topic or we might use multiple Pub sub topics with that partitions, they happen under the hood if you're using something like Google Cloud pubsub, the point is we have a messaging system here, and all of these API servers are forwarding the request to the pub sub system ,and then here we need subscribers of the pups sub topic



these are going to be subscribers to the pub sub system subscribers ~~I suppose we can just write topic to make the overwhelmingly clear,~~ and these subscribers are going to be the ones who actually do all the logic of going to the database, going to be of the allof the topic here, then notify other regions,





This kind of goes to the subscribers ,who are watching the topic, I think this is most of what needs to happen in this Regional system, let's talk actually about this other region thing that I just mentioned



~~that I said we still have to talk about it because it's not like super trivials let me let me get myself some space~~ let's assume that were like our system like I'll draw an arrow that comes kind of from the side, that's kind of saying like hey our subscriber is contacting the other region, where is contacting some part of the system, that's going to contact the other region ,and here we have like a big circle ,that's the other region, this is a duplicate version of our entire system here ,for the other region, but other region I suppose there will be multiple other regions how would we want this to happen



basically what we want is we want, these subscribers to notify the other region, that they should act if they are at this part of the system, like we have gotten a create a post, we don't need to deal with this Pub sub topic section in here, we just need to store the post and our regional database to forward the post to our regional topics here based on user ID hashing of the friends and then you know everything else works for the same in those respective region



so to do that it's basically notify the other region that they need to start here and probably have some kind of logic extra logic that tells the other region, you don't need to replicate this other region like you don't need to do this step of this part of the system you don't need to other region, I'm handling it I'm the region from which of this post originated, I will handle notifying all the other regions does that make sense



~~skating of of other region notifications that way that way if the create post message doesn't get infinitely replicated to other regions were full~~



of it exactly and every region handles its own contacting of other region based on Post in its own region post, that I've been created from its own region but the only things that I'm thinking is that we probably want to use pub sub again here we probably want to use another messaging service to guarantee that we end up contacting the other regions, as we need that stat guaranteed delivery, so I actually think that here we can just have another Pub sub system ,and we probably have like I'm actually give myself some space yearly grabbed this other region to move it we probably have some kind of like forwarding service ordering service forward service





and this subscribes to the pub sub messaging system receives the notification that it needs to tell the region to that we just got a post in another region let's start the entire Machinery that needs to happen, and there you go like you get to the subscriber's at this point it was subscribers who were who were over here does that make sense



the entire reason that we want to use pubsub is so that if one of these things fails here like imagine he's failed to score in the database or we fail to contact the other region, we will be able to redo it like we'll go get with the message will still be stored it won't have been completed and we can go back and get it and and redo these things ,but it's only works if all of these operation are idempotence, like if we don't care about performing them multiple times, and I think they are right, because you repeat the operation of storing a post in the database we can just over post to the host has like a unique idea that probably generated you know at this level



~~we would still we were trying to duplicate post if this is already been done like is viveros I've already been done but then~~ the feed the the feed sard's can actually handle that like as they see it as they're getting a post but they've already injected in their feed, they can have some logic that says like hey I can just dismiss this right, and then same for the notification to the other regions ,I think the other reasons can store liked posts they just handle and they've got or something like that and they know not to be replicated or the other regions didn't you just go back to them and like they've already performed all this so so it doesn't matter



the pups up system gives us replay ability of a messages which is really nice and then de-duplication can happen either at the pub sub layer in the in the other types of topics or be at the feed shard layer as well



yes and then I think I'm pretty comfortable then with with relying so heavily on pubsub here and with this system as a whole cool yeah that's I think that's such a pretty good design thanks a lock on it thank you would that I hope that you found this video informative and we'll see you in the next one
