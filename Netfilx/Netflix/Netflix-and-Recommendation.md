# Netflix and Recommendation 



---

Question today I want you to design Netflix



okay Netflix so from a high-level Netflix seems like a pretty straightforward service, users go on the platform, they're served video content, movies, TV shows, I can watch this video content ,is this what we're designing the high level, overarching netflix product or do you want me to focus on particular subsystems of Netflix



where does designing the core Netflix product as you said, so the overarching system that you described



Just clarify ,there a lot of auxiliary services ,that are part of Netflix like for authentication that users can create an account they can log on, they can pick a profile ,or the payment service users can subscribe to Netflix and pay with a credit card, do I have to worry about those axillary services or can I gloss over them reason skip them altogether



you can ignore those auxiliary Services focus on just a primary user flow, that being said one thing to note, is that by the nature of product, [we're going to have access to a lot of user activity data]{.mark}, that's going to need to be processed for the recommendation engine ,so you're going to need to come up with a way to aggregate that user activity on the website and be able to process it into save set of scores for the content



okay so you're saying that we're we're going to want to also kind of an able some sort of recommendation system ,in this this is something that operates what in the background maybe asynchronously, and consume this user activity data that we are gathering from users



so then I think that it would make sense to divide this entire system into two major subsystems, the [first one would be the core user flow, and what I really mean by that is anything that involves Videos ,so soaring videos ,streaming videos and I suppose I might you know also tie in storing user metadata and static content like video descriptions and things like that that will be one part of the system]{.mark}



and then I'm thinking of having the second part with would be the recommendation and generous system, and this one will be it'll be kind of tangential or related to the core user flow



does this make sense as a as a high level overview



just to clarify the core user flow as you described is a main system for which we care about latency , so that's the video streaming system, the recommendation engine no on the other hand you don't care about latency as much because you said it happens in the background



so you said for the core user flow , we're gonna care about the latencies, which is makes sense because users wanna stream video basically in real-time very fast and I'm assuming we also care about high availability for the system ,and is it are we also designing this set a global scale or is this only for specific region



or let's talk Global in this particular case and also highly available for the core user flow



~~okay so I guess what all put here is all ,i'll maybe put an arrow we care about latency is here late maybe I'll put fast latencies and we care about availability and this is a global audience ,but for the for the recommendation engine, like we said it happens in the background , okay so I suppose Global audience, Global audience~~



how many users do we do we expect to be using Netflix ,show Netflix I can just tell you this Netflix 100- 200 million as a conservative estimate,



I assume it spread out across the globe kind of equality.



last question that comes to mind is, people can use netflix on a lot of different clients, that people can use Netflix on their desktop computer , they can use it on the tablet, they can use it on their phones, do you want to me designed specifically for these clients

like, do I have to optimize for mobile clients



or even though netflix is used by all sorts of clients, don't focus on that, just focus on the distributed system aspect, we don't need to get into the detail about other clients



up so then that case I think just started, and I think that's like I said I'm going to divide this

into 2 sub-system, the video aspect and the recommendation engine.



maybe we can start with a video aspect or core user flow, that tired to the video aspect,

I should first decided what we're gonna store, like [what data we are gonna sotre]{.mark}. that's I think that like I mentioned before there's going to be ~~maybe let me write down his dad is~~ going to be video content, there's going to be a [user metadata, this is going to be weather user as watch the video, whether a user has liked the video, because i think you can rate video on Netflix, maybe how much videos the user has watched]{.mark}, that kind of data



I think there's going to be [static content]{.mark} this is going to be stuff like the names of movies and shows maybe their descriptions, thumbnails ,the cast of a particular movie that kind of stuff and then



[I think we might have also a fourth type of data ,for the recommendation on his and you mentioned that we're going to be your Gathering a lot of user activity, I don't know if what I have in mind for user metadata here, is like it quite exactly the user activity data that you were describing, so I think that we might on top of user metadata, gather much more granular ,like user events, like literally any click on a video, that kind of stuff in the form of log]{.mark}



there's going to be a lot more of that than just the user meta data like ratings and progression that you mentioned, so then I'll just capture them this fourth piece of data is its logs and we could throw in anything that we want hear, any type of granular event, we can just throw into these logs ,but for now I think I'm going to skip the logs section for a little bit because that kind of ties back to the recommendation engine,



we can start with these three pieces of data, which are more relevant to that core user flow. so if we do a little bit of estimation for how much storage were going to need to, because I guess we have to figure out [what Storage Solutions,]{.mark} let start with video, for the video I guess Netflix has Netflix doesn't have that many movies right, this is not like a service like YouTube, where any user can upload basically an unlimited amount of video content, Netflix has a very limited amount of video content that's

picked by them ,I think is it fair to estimate the Netflix is like a 5000 movies



so let's go with something slightly bigger than that lets his goats with 10000 just to be safe in terms of our estimates



so we've got 10K movies , standard videos and we need to store 10K videos, now videos if I remember correctly from Netflix ,Netflix has multiple like different resolutions for videos Right video quality, i think there are different subscription Plan , you can see videos in standard definition, high-definition I forget if they've got like 4K or something, so we need to store every video in all those resolutions,



should i assume that we've got three resolutions or which ones should I assume



let's just go with standard and HD the standard and HD, so put 10K videos is got standard definition and high definition, let's see a video on average is probably like an hour in length cuz we have movies that are up to two hours, we have TV shows that are just like 20 minutes per episode, 30 minutes let's go with an hour, as an average video length ,and now we're a video in standard definition is probably like and I would say like 10 GB per hour, tensions videos that are 1 hour average, and then I think 10 GB per hour for standard definition and HD would be like a double standard definition like 20 Gigabytes does that seem like a fair approximate



20 Gigabytes per hour in here what I'm really saying by per hour is like ,each hour of video content ,takes 10 GB in SD and takes 20 gigabytes in a HD, then he's like 30 gigabytes for every single video ,and if we've got 10K videos of 1 hour each, let's say 10 k * 30 GB that is 300 TB ,~~actually that much data like I was expecting quite a bit of Netflix that it's almost purely video content~~, ~~and so they're going to have to sort of pretty low~~ compared to something like YouTube or Google drive, it very very palatable I'm thinking that we don't need to optimize or do anything like super fancy with our videos for its solution, and we can probably get away with just storing or video content in like a blob store ,like at S3 or GCS, does that seem reasonable,



so then I'll write here, the first in a storage is going to be what would go with S3 Amazon S3 ,this is going to be our videos storage, and this would probably be in a replicated maybe across regions or something like that, and I suppose we will get into the actual serving aspect of this video content in a little bit, but that's what I'm going to go with for storing the video content



then there's user metadata and static content we said we'll do logs, later for the static content, we said we will do logs later



[for static content,]{.mark} I think that we can probably like static content is relevant to videos, right, so there's no more like there for every video there static content and static content is this text ,so it's going to take up way less storage than video, so I guess it is static content is bounded by 300 terabytes I think ,and it's not nothing super fancy, so I think we can probably sore this in a ky value store or relational database, I think I'm not going to put too much effort into this is



for user meta data, I think that we can do we can also estimate, cuz now I'm starting to think you give this video of content takes only 300 TB terabytes, we might actually have more user metadata then video content, cuz we got 200 million users, so maybe we were going to have to do some things for the user metadata, let's see what's the estimated



so we have 200 million users ,we're going to be storing user metadata about these two hundred million users, probably / video and add the video level, is this user metadata I said is going to be something like whether user has watched the video ,so that would be kind of like how long or how much of the video they watch, so how many videos do we estimate a user will watch in their lifetime, cuz we're trying to estimate how much storage we need is for all of user metadata across all of Netflix, in perpetuity, so let's assume maybe that a single user watches at a one video or two videos a week on Netflix ,if user watch two shows a week, let's say 50 weeks in a year there 52 let's run it down to 50 ,to a 100 show per year, let's take an average lifetime for a user like 10 years on Netflix ,does Netflix has only been around for 20 years and let take 10 years, a hundred show per year, that's a thousand shows in a lifetime so we're going to be stored per user a thousand like user metadata on a thousand videos, and how big is this user metadata it doesn't seem like it's that big like if it's the thing you know whether it's watch them how long of the videos been washed were, talking in the order of bytes like 10- 200 bytes per video



Does this seem kind of accurate? are you thinking that we might have more user metadata? yeah let's go with the hundred bytes per video for user, just because we don't know what kind of overhead, we might have with the storage medium





let's do a hundred bytes, then 100 bytes per user or per videos, are am there are a 1000 videos per user we said in a lifetime, so 100 bytes * 1000 ,that's going to be what's 100 kilobytes, 100 kilobytes per user, I guess ,then we have 200 million users, so we're going to 20 TB of user metadata, so we are we are like I said in the same order of magnitude as with the videos storage , for it or the amount of space that videotape



so that's interesting again the reason I'm surprised is because ,I was thinking that your Netflix is a video streaming platforms of video is going to take up a ton of space, but I guess it's just not like YouTuber or Google Drive, where you can upload an unlimited amount of video, all that being said, I do think that for the for this user metadata ,we can probably just store it in a relational database



~~we're likely going to want to query this data often right, let's see do we want to do anything fancy there, I'm thinking that for Netflix that you go on the platform, a user my update their user metadata in the background or we might need to request the user metadata, but I feel like anything that is latency sensitive, won't be an expensive operation to run, it'll just be like read user metadata from the database ,on the other hand anything that might be expensive, like compared using metadata across a bunch of different users and all that stuff that would only happen if you were running some kind of analysis in the background, like imagine a systems administrator we're running analysis on all of Netflix user of metadata or something like that,~~



so what I'm getting at is because Netflix users are by Nature isolated and Netflix users are the only people for whom we care about fast and latencies, I think that we can just shared whatever like relational database store this user metadata at the user ID level, so I can imagine we have here you know a relational database for user metadata, I think that we can probably shard it into a few shards maybe 5 - 10 shards and each shards has 2-4 terabytes of data and the this shard would be based on the user ID



then for the static contents like I said, this static content I think we can just put in another relational database, so maybe I'll add here this is going to be static content ,in a relational database ,



okay so that is really easy the storage aspect, I guess it's like I said it's less complicated than I was expecting, what I think will be complicated will be actually screaming or serving this data, and this static data ,and not the other user metadata, but the core Netflix service right is being able to watch videos, and like you said we care about very fast latency is globally ,and if we've got a 200 million users, maybe it might make sense to try to estimate like or [bandwidth consumption]{.mark} at any given point in time ,and to see like how much we're going to need to how much data were going to need to serve





does that make sense could I do that ? so I write down this here, so let's see, we've got 200 million users right ,hundred million users are watching videos I suppose they're not going to be all watch at once, we can assume probably that during your peak time, maybe 1% of Netflix user, they're watching a show ,maybe like if they're so those are very viral ,super popular maybe he's got 5% of the global Netflix user base ,watching like this video at 5% would you want like 10 million, if we're 5% of 200 million ,10 million we're going to assume 10 million is the peak traffic ,10 million users is the peak, and if he's got 10 million users watching a video, in let's say HI definition has to be conservative, 10 million users need when 20 Giga bytes per hour, so 20 Gigabytes per hour is how many gigabytes per second, estate of their 4000 seconds in an hour ,to round things up ,and make things easy to calculate 4000 seconds, in an hour or so 20 gigabytes divided by 4000 is 5 GB divided by 1000 ,5 megabytes per second, to 5 megabytes per second and this is per user and we've got 10 million users to 5 megabytes * 10 million ~~* 10 is 50 times a million you add six zeros~~ that is 50 TB to per second bandwidth consumption



~~does that seem accurate because the peak amount of bandwidth, consumption that we might expect on the Netflix service , 5 megabytes per second -- 5mps~~



,so okay so 50 TB per second that ,I'm not like super well versed in in video streaming and all that but that seems like a lot that seems like something that is not handleable by single data center or even a few data centers, so I think that we're going to really want to spread out this load across probably like a ton of machines ,and even more specifically a ton of locations, right cuz you don't want it's not about a single data sent for having a bunch of machines, is that still like one bottle neck at the data center level ,as far as bandwidth goes we want this to be spread out over or across a bunch of different locations



so they know what we can do what comes to mind would be to use a [CDN]{.mark} for the video servicing, we can use a CDN like cloudflare and the CDN the key point is that it has to have a lot of points of presence , at a lot of Pops, and these pops probably consist thousands or hundreds of machines ,that way we can bring down and I think we've got a CDN with a thousand points of presence, we already bring down the bandwidth consumption to the order of gigabytes per second, and it's the point of presence have themselves with clusters of machines than we can bring it down to the to the order like megabytes per second, and this CDN basic allows this insane amount of a video content to be consumed by so many users at once



that does that make sense is that on the right track, looks good how do you make sure that the point of presidents have the right data locally?



You definitely need some caching ,cuz you don't want request, just have to then grab the video content from our or [blobstore]{.mark}, you're back with the same issue ,so let's say we have our CDN, I'll put the CIA hear, you got CDN here, and it's got a bunch of Pops points of presents ,and you got users, here and white here is that users who are requesting videos from our CDN, our points of presences here have to have video content cache and so what I'm thinking is that [we can have like a service that lives in between the CDN and the blob store that basically asynchronously populates these pops with the video content that they need to]{.mark} have ,so you don't know if Netflix is releasing a new movies ,than these the service like a cache populator will make sure to have these new movies yet ,sent to these points of presents ,and I'm probably like prioritize ,important movies, deprioritize unpopular movies does that kind of makes sense by this



~~will have a cash populator yeah so this is I think how we're going to achieve the this is going to interact with this with our blobstore~~ I think this is how we how we'd achieve the video streaming in this this really high bandwidth consumption



this is perfectly Reasonable, it turns out Netflix in real life solve the issue by partnering with internet service providers themselves, instead of having instead of using a classic kind of CDN, they partner with the Verizon the AT&T of the world and injector special cashing layer crafted by Netflix at locations that are called internet exchange points, [IXPS]{.mark} and these are kind of the counterpart to the point of presents for a CDN and there is Thousands tens of thousands of them across the world, if not more than that and so that's it behaves it's actually like an optimized CDN, which allows for much cheaper bandwidth for Netflix and the uses obviously have much lower latency is well



an optimized much more specialized version of what I described here, it's good to know it up for the for the core user flow where



[I suppose we need to talk about how we're actually going to be updating the user metadata]{.mark} ,and even getting the static content to hear anything and the same way that we have users who are requesting video content from the city and from the wall store ,eventually we're going to have in a layer of of API server ,



our users here who are [requesting you know they're requesting static content]{.mark} and user metadata from our API servers ,were probably going to have some load balancing happen beforehand so the load balance here ,I guess we can probably do round-robin load balancing for all of our request, maybe we can we can already load balance at the user ID level ,cuz we said that all of the user metadata that a user is going to want, is going to be specific to them, maybe it makes sense to have load balance according to user ID, or if not if we just go with round-robin then our API server is here ,will then go to the appropriate shard based on the user ID, in the request, but nothing too fancy hear, all the right load balancer



one thing I didn't want to say is that [if we're dealing with this 20 terabytes of static content we can probably cash or static content in memory]{.mark} at our [API server level]{.mark} ,so we probably don't even need to go to the static content database ,we need to go maybe every so often you know what maybe like everyday the cahced updated or something like that ,and to be honest even for the user for the user metadata, I'm wondering if we could do just cashing at the API server level in which case we would have to do the user ID based load balancing ,but some sort of like write through cash policy



does that make sense and at this point, now I do think that we've taken care of the core user flow I think that now we can move on to this recommendation is that okay or do you think I'm missing something



no I think we can move on to that that makes sense ,so for that I guess give myself a bit of space here we said that we might want to gather more granular user activity, so like logs and thinking that these logs, they're going to be things like I guess it doesn't make sens[e do you think it makes sense to have these logs be what gets fed into our recommendations]{.mark}

[engine]{.mark}. or maybe not he's not necessarily the recommendation engine, but the system that backs it? like yeah we have some sort of mapreduce job that asynchronously consumes all of these logs ,that are stored somewhere and then spits out some sort of more meaningful data that is used elsewhere in her system is that kind of like what you had in mind for the recommendation engine or resist something else



that is what I had in mind but I'd like more detail on number 1, how do you store the logs like where they should be stored, what technologies to use, and then what the map function reduce might look like? I'm not asking necessarily for with the truth is and what Netflix exactly does, but just an idea as to what you could



do okay so since we're going to be performing mapreduce jobs, on this data, we're going to need a distributed file system ,so I think we can store our logs in a distributed file system ,like Hadoop distributed file system,and basically like you deserve a user is here and they're they're sending logs, all the time and gets stored into hdfs, and then we've got this asynchronous mapreduce job ,that takes the logs, from the GFS and actually processes the data ,of those logs so



if those logs look like, let's see along would be kind of like raw data, right so along might look like as a [user ID,]{.mark} user ID, it's got maybe an [event type]{.mark}, this would be something like ,[a pause or click on the screen ,or Mouse move,]{.mark} whatever granular events, Netflix thinks might lead to meaningful data,some something that can predict engagement for that kind of stuff, we want to track, event this would be probably like an enum type in capital letters and then probably the video id, that we might store one, I'm sure that we might store other stuff in these logs like maybe the region other things, but this would be like the core the core values that I would expect in these logs





does this make sense if these are the logs their sored in the hdfs then [we feed them into our map functio]{.mark}n, [our map function take these logs as inputs, and then it probably Aggregates them as a user ID level,]{.mark} because you said that this is a recommendation and then you said that we probably want to spit out like a score, or something for the user or each video, so I think it makes sense to aggregate to have the map function output intermediary, key value pairs that are based on the user ID ,so do something like you know





[let's see where this goes into the map]{.mark} ,and the map split out something late user ID, or not even user ID, is have the actual the key is the user ID, so like UID1 and this may be is a list of tuples where [the tuple are like the video ID and the type]{.mark} ,this would be a valid ID 1 pause ~~Danny would be let me get myself a little bit of space~~ it would be very UID 2, play this kind of making up the events here, but you got the idea right so you've got these all of these key value pairs, that are indexed at the user ID level, and he's got all of the events with the relevant video id for each user, and then that gets fed into our reduce function right in our reduce ,or reduce here





I don't know exactly how this might work ,in a real system like Netflix, I'm assuming that they might have you know machine learning models that are getting trained ,on this intermediary date, or some sort of data pipeline that grabs the date on the middle of the reduce function ,but eventually you might spit out the final score that you were that you were looking for, [so this might be something like a user ID pointing to a video ,and a score]{.mark}, or maybe it might be a user ID pointing to like a stack ranking of videos, like down here to have UID1 pointing to the V1 ,V2 V3



or you might have you ID anyone wanting to the tuple, v1,score , and here [you would have a bunch of these scores each video ID ,d]{.mark}epending on your depending on what your data science department might be looking, for depending on what data ,you think is Meaningful for your recommendation engine, and then you could write your reduce function in various ways



does this make sense a reasonable kind of approach recommendation engine that I can think of and I think of that as captures everything that we wanted to build here yeah sounds good thanks Clement wood that I hope that you found this video informative and we'll see you in the next one



![StoreVideo (Admin API call) Centra Video Storag e store: 33 GCS) Relational Database (postgres  MySQL) user Metadata (ratings, progress) Sharded based on userld Machine Learn Relational Database (Postgres MySQL) Stanc Content (cast. duration. views....) MapReduce Job Content Scoring user Activity Storage (HDFS) IXP / Popula 'XP / Pop Cache Populator Netflix Video Cache ISP IXP (or CDN POP) IXP Pop Populator API Servers Netflix Video Cache ISP 'XP (or CDN POP) Servers Load Balancing Round Robin Servers Cache ](../../media/Netfilx-Netflix-Netflix-and-Recommendation-image1.png)

![](../../media/Netfilx-Netflix-Netflix-and-Recommendation-image2.png)


