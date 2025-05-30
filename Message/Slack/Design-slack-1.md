# Design slack



---

Design slack



Okay so there's a lot of things that you can do on slack, this one first and foremost you can communicate with other people, send messages ,and I guess you can send private messages send one on one chats, and within send like group message and channels, within the channels, you have private channels ,public channels, then part of this this messaging functionality ,there's a lot of other stuff that you can do on slack like create and delete channels, update their setting, invite people to slack channels, within a group or organization, what are we designing exactly here



so I want you to focus on the core communication or messaging functionality, that involves both [the one-on-one direct message and group channels.]{.mark}



but I don't want you to worry about channel settings for the extra functionalities that you mentioned.



okay so then I guess I'll write down just messaging as the core thing ,that we are handling messaging, but in slack, if i remeber correctly from my from my own usage of slack, you have what are called organizations ,like the slack of company, like organization for algoexpert, do you want to handle this concept, and for channel, I guess live under an organization normally, there's this concept of private vs public that I mentioned before, do I have to design my system with that in mind



each [Channel belong to a single organization and for private channels let's just focus on users in a channel being access control, we can just forget about the concept of private versus public]{.mark}



we do have organization, I'll jst shorten it as Org,in that and in that you have channels multiple channels or potentially multiple channels, and then you said okay we're just going to worry about what users are in a channel, as far as access control, the we don't need to worry about this concept as private vs public, it is simplifies things.





as far as the messaging functionality, when you go on SLAP, you can obviously send messaging to people, you can red the messages, there are bucket per channel, you open one channel or one direct message channel, you see all message in there, but you can also notified when you're on the slack app of unread messages that you have ,like you have a channel for instance as a message, it's bolded I think, so you can see that and I'm not mistaken, you can even see people who tagged you in that channel ,like how many people tagged you , how many times you've been tagged. do we want to design our system to accommodate these things ,like unread and the number of mentions, or so we just worry about sending messages



Yes, you do want to take care of this, and I want to mention one other thing that will call cross-device synchronization and basically she have the slack app open on your laptop, and on your phone and both apps for showing that or a single channel, one channel is unread, then the same channels is unread, if you read message in one of one of your two clients say your laptop, your phone should automatically register that message has been read and that channel should no longer appear as unread.





that I see okay so then I'll write down the fact that we do care about on the unread messages ,I'll just put it as unread and we care about other thing we mention -- tag, but then we care about this cross-device, that makes sense, because when you have phone, it

does update automatically



okay so that's probably tell me we are going to need some sort of you know instant notification system probably , probably pub-sub system, but i can talk about it later, we probably going to need that for the messaging functionality anyway, but few more questions before I actually dive into the design, speak of the application. because you mentioned like you have your phone, your desktop,Do we care about designing clients? like mobile app or desktop app or we just taking care of then backend



you just need force on the backend system,but you do want to keep in my kind of communication between the client server happens



I guess I'll just put backend and this implies that it's also like backend integration, how we communicate with the API I suppose, Let' see if I am missing anything, when you are sending a slack message,



I guess when you're sending out a slack message there's a lot of functionality with to a particular message, you can send code Snippets, text blocks, emojis ,you can even like pin messages in a slack Channel ,or save them, should we handle all this extra messaging functionality or no



No, you can just treat the message as pure text for now,but we do want to design this in such a way that it's extensible, may be in the future if we have enough time with you going to handle pining or saving message. but for now , definitely don't worry about it.



it okay so then I guess I'll just put basic text, I won't worry about like pinging or other stuff.



my last couple of question has to do with the scale of the system, cause I think I have the functionality, like in my mind now, that we're desiging,but how many users use slacker, how many users are designing for and I guess go even beyond that, when we're talking about an organization or channel, how big can those get? how many users can be in those?



so slack has about 10 to 20 million users today, so let's just go with 20 million, that's the upper bound, and for orgs, it's say the single largest customer, say it's a tech company with 50,000 people on the same slack organization, and we can also assume that the largest channel will be 50,000 for largest Channel



if I realize that like you said it's not every worker every channel that has 50k, but you're basically saying the upper bound is that so that's what it was I designed around.



And then also since this is a chat application, assuming that we we obviously [care about the latecy, you want low latency, we probably care about high availability]{.mark} is serving so many users is that correct?



so yeah we would care about both of those things, but for the sake of being a bit more focused I don't want you to optimize for availability, I want you to focus primarily on the latency and core functionality and making sure that clients can really have that user experience that we want.



The clients can really have that user experience that we want I think the functionality I've I've marked down, and I have in mind we're definitely going to support that ,but then I'll just add latency is what we're focusing on, that kind of simplifies things for the Narrows things down, and finally are we designing this for a global audience I'm assuming so right so



We designing this for a global audience, so in general you would be ,but for the question for this question in order to make it more digestible it's focused on single region, so don't think too much about this aspect of design.



okay okay so then it's really just latency as far as like systems characteristics.



okay so then the answers most of what I had ,almost to where I was wondering, I try to think how I am gonna approach the system,



I guess if the messaging functionality seems kind of straightforward, but I feel like there's a little bit more involved than it seems, because it's not just sending you a message,and you are seening it pop up on your screen, there's also like booting up the slack app and seeing all the messages that were previously there, so there is both this instant messaging aspect and seen stuff happened in real-time, but also be able to look through old messages and old channel, all channels, and plus i guess on top of that,there is the whole like unread messages mentioned so I guess [what I'm getting at is that maybe we can divide two parts]{.mark}





[the one part that would be the instant messaging, the real time aspect of the chat application and one part that would be what happens when you when you load slack and this part will interact with like persistent storage solution of some sort,]{.mark} Does that kind of make sense as a tentative plan?



~~then maybe I'll just I'll just write it up here so they don't forget we'll have number one persistent storage , it's what happen on application boot.and then there's real time what's call it real time SG for messaging~~





what's probably makes more sense to tackle first, is the persistent storage aspect, and like I said this one is really tied to what happen when you open slack,when you open the desktop app where you open a mobile app, that's the first thing you open slack, the first thing that you have when you open slack,that you see all the channels that you're part of,

that you're apart of right and that's probably where we're not probably, that's certainly where you see if a channel has unread messages, or how many tags that has it, so that means, that you need to be able to get all of the channels, that the user are apart of ,so what that tells me is that, we are almost certainly, going to want to store, in a persistent storage solution, and I'm really thinking a SQL database, is we're going to be query this a lot, we're going to want to store every channel, and more specifically probably every channel, per user ,or every channel user pair that makes sense





I would work on the table would be I guess that's just the information that that's necessary to figure out what channels are user in



I think we do need ,we probably need a table for every channel to organization pair, cuz you need to know what channels are part of an organization, right so you need that mapping, but you also need them a mapping of every user in a channel, and you're going to be updating these two databases, if we even think of functionality that you said we don't have to keep in mind right now, like editing settings, and we're going to we wouldn't need to edit these two separate Concepts, I think that having two separate tables for that would make sense



if you want me to write those tables out, or like write those table's columns or no,



I think these two are fine but some of the next tables that I assume you'll be coming up with I'd like you to write the schemas for, if they get complicated



just to iterate for those two table, they would be very simply ,because just be [uu id, unique ID for the channe]{.mark}ls, for one column for the users, they would also have a unique ID and the other table would be unique ID for channels, and unique ID for organization, so

this would be the first two.



now the next obvious table that would be SQL table that we're going to need is going to be for all the messages, because even though your messages come in, and you see them in real time, and so we're probably going to need a pub sub system like I mentioned at the beginning, you need to be able to retrieve those messages at any given point in time ,right when a user clicks a Channel, or did he even on page load, I guess I'll decide on that in a second or perhaps you can give me guidance, there but regardless we need a table where you can get all historical messages for a channel, so yeah we're going to have one table that's going to be really big, where I think every row is going to represent a message, to this would be the historical messages table ,I supposed to hear I could write things down right like that could ride the column down



this would be message or historical messages, this would be the table and every row would have no unique key or unique ID ~~let's call it ID, and I guess it's your I will write the type of the type of all of my IDs, I am going to to call IDs are unique IDs IDs~~



but we are going to have for the message, message belongs to a channel, perhaps even an organization ,I'm not sure that we need the org ID, here but I think we can add it if we want to, it's kind of trivial, but for now we'll just put [Channel ID ,so this would be the ID that the messages in]{.mark}, you would need a [sender ID,]{.mark} so this would be the user ID that you need ID of the user who sent the message, we would have probably [like a timestamp for when it was sent ,"sent at"]{.mark} probably know a body this would be a string, and any other information that we want to have about messages ,we would score in the table does that make sense



might just get myself a little bit of moving everything out of condensing everything together, but so yeah that's our first or our first like big table ,I guess the other two are also relatively big ,butt every channel ,but this one is really like a huge table,



now see you load the slack app, I don't know I think like unload on slack, is it is it fine if if we load messages only ,when you click on it ,basic when you make a query, know the last I don't know 50 or a hundred messages of a channel from this table, when the user click on that channel, is that seems in line with the other system wants right?



yeah we definitely don't want to fetch messages from all different channels, when on on boot up as you call it on ,cuz that would that's that would just be a lot of data on it ,most of it is unlikely to be accessed by the user, a lot of channel, from my own usage of slack I rarely go on like 90% of the channels in my organization ,so okay then that's what we're going to do the idea ,[is that you would only query this table when you click on a channel]{.mark} and this would be going to go the API call to query this table, as a get messages call or get messages by channel ,[would give you the paginated API call]{.mark}, and you would probably even specify the [page size,]{.mark} and so on and so forth, now I'm trying to think, so obviously we're going to need also some sort of load balancing and probably some sharding for this but I'll get into that in a little bit



I want to just make sure that I have the entire storage Solutions are picked out or be fine, the thing that I'm missing right now is the knowing if a channel has read or rather unread messages, and also the other number of mentions that I load up the page,and I have a channel that has unread messages, how do I know that? what does that mean? that means that the the latest messenger or however many messages have come in that channel ,came later or after the last time I read it



[so basically we need to keep track at the user level, of when a channel was last read ,]{.mark}right because the only way that you can know still unread messages ,if it is if you have some data point ,that tells you when you last read that channel, does that make sense ,so in that case I think kind of think, we need we need to know, that we need to know, [the we need to have a table that stores the last time a user read the channel]{.mark}



so basically I think we we were going to have a table, that's going to be like last like read receipts ,is it like Last Read receipts, on this would not be used said to give a read receipt to another user, cuz I don't think that's a thing in slack, to know if you have red or not read a channel, is this will be at a table, where you have column, so I guess I'll write it here I'll put [read receipts]{.mark} as well call it read receipts and this channel would have to be pretty simple would be ID, let's add the org ID just for the sake of having it ,Channel ID channel ID I suppose by the way I forgot if I mentioned it at the beginning ,when you get all the channels ,you would you would issue an API call like getting my channels, and that API wood query in every channel for the current user ,and those would naturally fall under the current organization that you're in



so here we have org ID ,channel ID, user ID ,and then what are we what would we have will just have [this basic have the last red or last seen, in]{.mark} this would be a timestamp, I have this would be a time stamp that says when was the last time that you accessed this channel, that you read this Channel and we would write to this database, whenever you click on a channel, to those the client would have liked you know a mark As read API call ,that it would hit or that it would call that would update this the row of corresponding Row in the table does that make sense



now this is only part of the problem or part of the solution, rather because this tells me when the last time I read a message, it doesn't tell me yes that means that there's an unread message or read message in that channel ,so what do we do well ,maybe, maybe we on boot ,so he said on boot we didn't really want to get all the messages ,right the maybe it makes sense on boot ,to just get for every channel, that is that is like in that responsive get my channels ,[you get the last message, the last message in the historical messages table for that channel, and you see if the sentAT,that time stamps came after the last seen timestamp, the only thing is it does seem like it, would be doing that call for every single channel]{.mark} ,would be kind of wanted to avoid them ,since you would have to take the most recent timestamp, I don't know if that would be like an ideal way of doing it





another way of doing it is maybe we have [this another table(or column ) ,literally another table, that keeps track of the last activity in a channel,]{.mark} ~~to basically be like yes it is it like ,this other option would be, imagine~~ [we have Channel last activity ,maybe, Channel last activity,]{.mark} and this would be also a simple Channel or simple table ,sorry that would have you know ID, org ID, channel ID, channel ID ,but here it would have the last message, last message time in this would be a time a timestamp, that's like the last seen, and so when you when you boot the slack app, you request for all of your channels ,the row responding to the channel



[In this in this table and you compare the last message time to the last seen time ,and that tells you if it's on red or not, does that make sense]{.mark}



It just seems a little bit simpler to have this extra table, kind of like separating concerns a little bit, [then to use this this historical messages table to get the last message for every channel]{.mark}, maybe there is one edge case that I might be missing is you know the channel last activity is from you, and with case like it cannot be unread by definition



maybe we could have like a user ID for who the last person who sent the message was and we compare that you're if it's you then you just disregard that ,but yeah I think I think maybe that makes sense add, what do you think we could do that or you can use the fact that the [read receipts table gets updated, whenever you post a message to on the channel right cuz it when you write a new message to channel, you're obviously reading that channel]{.mark}



so next. OK and we don't even need that extra thing, okay so then I think I'm going to go with this the last thing that I'm missing about the on boot is the concept of the mentions ~~limited separate these tables out a little bit that we see things more clearly~~ the dimensions are there equivalent to read receipts ,the only thing is you need we need, to know you need the number of mentions in slack right



yes so per Channel you need to know how many times you've been mentioned ,that you haven't read, it unred mansions for every channel unread medicines okay



so then I'm trying to think the first of all that tells me that, in a given the message, [we need to somehow store some extra information about late weather or not there are mentions]{.mark}



so this might be kind of tangential, but when someone will be sending a message, either the client will pass in in the body of the send a message, request the mentions your clients side mentioned parsing thing, so to speak ,or maybe the backend would parse it and you know store the mention separately,but we definitely need in this message table table to store parts from the body ---the Mansions does that make sense





I'll put mentions here and you know obviously, you like this there might be some optimization, is that we can make here do you want me to focus on how exactly these motioned are stored, like exact format or type in the table ,or should I just focus on a from a higher level how we're going to get that mention count



want you to focus at a high level on how that's going to work, because I think it old potentially change how you store them



I'm trying to think I think we need something similar to the read receipts, especially if it's unread mentions ,then I think the concept like the functionality that we have here with us read receipts table, just makes sense ,so I'm kind of busy thing is it's the count ,so what do we store more like every single mention, and you know you query for the number of mentions, or maybe not maybe you just store account of mentions, the maybe yeah maybe you do this ,you have a table that is ,[last unread user mention count,]{.mark} last unread user mentions count, here very similar to the read receipts, so it's almost like, ~~I can copy this entire thing here so paste the same structure~~ except instead of instead of a last seen its count ,right it's count is an integer ,here I guess when someone writes a message that happens to have a mention for a particular user ,we also write to this database or to this table right in the update incremental account, and when a user marks of channel is red then we update discount to 0 does that make sense



so basically will whatever we store here in these mentions ,I guess I don't even think that it really matters, how we store these mentions here ,[all that matters is when a user marks of this channel as red ,you update count at zero , when a user mentions another user ,you update this table in particular you know the relevant row ,and you update count by 1 or by 2 or however many mentions of that user are in a message, is that okay]{.mark}



so I think that we have all of the functionality that happens on boot up, and this is our entire of your persistent Storage Solutions ,[there a lot of tables and like I hinted at it probably has two API calls, that write to these tables is the send message and marked as red sending a message will potentially update this user mentions table by incrementing counts and marking things as read ,will update the read receipts, and potentially this making count be zero]{.mark} ,and obviously also the channel last activity ,and sending a message will update the messages table, now before I move on to the real-time messaging aspect



pub/sub subsystem that I've been hinting at so much ,we definitely are [going to want to shard of his database]{.mark} and we're also going to want to have some load balancing in the mix because slack is big ,as we got 20 million users, we got also like a very large organizations the one issue that we might run into here, is hot spots ,and if we really care about latency and what we obviously do, [then we don't want to run into hotspots ,so the way to handle these hot spots is going to be to shard all of these tables ,]{.mark}and all these different are persistent storage solution is going to have to be started, the things that I'm worried about those that, like let's say you shard based on organization or organisation size ,like you know Amazon which is a big organization in once shard,that you put no algo expert and 10 other smaller companies and to another shard, like slack organizations are likely the you're always in flux right, it's possible that tomorrow Amazon lays off half of its employees and suddenly there's far fewer activity or far Less activity in the in the their organization



or algo expert triples in size, so I'm getting at is that where we can just naively rely on the same shard at all times, we need some kind of rebalancing ,[kind of smart sorting that that rebalances what organizations are or in what's shard if we go with per organization or per organisation size starting approach]{.mark}, does that make sense?



~~or I am I off track here ,I think that makes sense to not off track ,one thing we do have to note, is that the organization's themselves aren't that huge, we're only talking about 50,000 people ,it's not Millions not kind of scale than Facebook, Uber ,, so it makes sense for us listen to be able to fit on a Single Shard~~



~~my one fear there is just that an organization can have a lot of channels, but I suppose that's still it kind of bounds down is the scale of those channels, since those channels only have like I'd most the number of users in the organization , I guess regardless we can fit probably one organization, or like this is a biggest of organizations can fit in to the shard , the point of the matter is that if is this circle here is our database, and here I'm taking into account basically like all of the tables that I've written above here ,this is what he started this is started ,but it started with this sort of smart starting approach, where did these the stars are rebalanced accordingly and we probably need some auxiliary service maybe with maybe all i Mark hear,that that does the smart starting asynchronously right~~



and this could be like key value store, by this could be a very highly available key Value Store, like a CD or zookeeper, that map organization ID to shard, and that way clients know what shard to hit, and I guess not clients cuz we would probably have like a set of API server, if here we've got clients ,right these little white circles here are our clients, so literally you or me ,if we're loading the slack mobile app, we make requests to let they send a message, or to get my channels, get old messages whatever right you've got a bunch of API servers here in the middle ,and here,there's some load balancing before the servers, so [I'll be our clients are issuing requests to the load balancer, the load balancer than issues or redistribute or re-balances these requests to the API servers,]{.mark} and here we can probably just do round-robin or something ,it doesn't matter which API server we hit, however the API server is when they go when they go to the database, they need to know what they're going to ,so say user ,if a client is from one particular org, they need to know to the course ,they need to go to the corresponding org shard, and to do that, they consult with this smart starting solution, to know that, does that make sense





okay so let me see handles everything related to that, first part of the system, that the non real-time part ,right now maybe we can do the real-time parts ,[of the real-time part would go let's put it to the right here,]{.mark}



what is the real time part exactly will I send you a message ,we're both on slack, if you're not on slack ,and I send you a message next time you go on slack you'll be one of these clients here and you'll just go through this entire system ,and we covered that, if you are on slack and I send you a message you need to get that in real time ,so that means that you are you'll still be one of these clients, I know you'll be here you'll have loaded and Boot It Up the platform, but then [you need to be basically listening to a real-time messages, and when I'm sending you a message, which is also going to go through this system]{.mark} here, cuz I'm going to be writing you are historical messages table and all that, [I need to somehow push that message to you]{.mark}, right so what I'm thinking is that these API servers here ,part of their job or maybe you know they have siblings servers ,I'm not sure what part of their job or maybe you know it's a clustered API servers, and you know after they write to the databases, they pushed to the real time part of the system ,this is that we'll go to a pub sub system



here we've got a pub sub system and I think you know we can we can just go with Apach kafkas , we're going to have a bunch of kafka topics, but them rectangles like this ,right you got a bunch of Kafka topics ,t[hese Kafka topics are in charge of getting all of the message being sent by clients, are by the API servers, and coming from clients, and then pushing them, basically pushing them all the way back to clients all the way back to the correct clients who should be listening to them]{.mark}, but I think we're going to have some sort of intermediary set of API service.



~~a red circle chair against servers~~ [and the API servers are basically subscribing to the Kafka topics]{.mark} ,V ~~servers are subscribing to the cost to fix it I guess this is kind of like a two-way Arrow if I directional arrow and~~ [then they send the information back to the to the clients]{.mark} and I guess what I'm saying send the information is on the service here when I say send the information [I mean like we clearly have to have some sort of long-lived connection]{.mark}



[so we can do a long live CP connection websockets between the clients and these API service]{.mark} that's how we're going to get that instant notification of a new message. Can I keep going or know that that makes sense?



so then I think it's like if it is as simple as this ,all know cuz Okay so in the same way that we had to shard a database here, [we're going after shard these topics, because we are not gonna send every single message to the same topic or a random topic]{.mark} so I think we can do as we can we can probably follow the same smart starting strategy ,we're like these API servers notify or push to a topic based on org ID, now based on the organization that corresponds to that topic ,and we follow the same smart writing strategy, to pick the topic ~~by the wildest put PS topic here~~ and then here we need one API server or one cluster of API servers to subscribe to each topic



these clients so I guess, if this API server is subscribing to like,( let's say this is pops up topic is algo expert, org topic ),this API server is subscribing so that ,what is this API server is pushing all like algo expert messages here to this topic, and falling the smart shard strategy, what we can do is we can have here we can have a load balancer ,we have our clients, or [clients are streaming from this right part of the system ,the screams go to or the longest connection go to the load balancer]{.mark}, [which itself is communicating with the with a smart starting solution, to know what API server, it's basically like a sign to a client]{.mark} and it ~~takes that basically says oh smart sharding, says that the algo expert org is this topic with this API server is subscribing to~~ ,so I'm going to redirect client here within the algo expert org, I goona redirected this or route this long live TCP connection to this API server does this make sense



I know that I'm kind of rambling a bit, there's only one thing that's that's missing and it's the [cross device synchronization]{.mark}, you haven't really talked about how this gets solved



I have my mobile app and have my desktop app and there's an unread Channel and I open it on my desktop and it suddenly gets red on my mobile is that what you mean



~~okay just let me staying here this captured like sending a message and receiving it instantly but here you're saying that you want me to receive my own read receipt instantly basically~~



maybe I think the solution is we use this exact system and I just threw out it's just the one we push messages by the way word messages a bit confusing here, we are talking about slack messages, we are also talking about pub/sub messages



it should be like notifications when you push your notification to a pub sub topic ,that notification could be a slack message, or it could be read receipt,



if maybe draw out here, let's put it up here a message, I got a pub sub messenger notification ,has a type, right it would have liked type would be either message or for read receipt, and probably we could have even other types, I can't really think of anything off the top of my head, but your this is extensible ,and then you would have a bunch of other information like you're probably ,user IDs the person who sent this ,org ID ,org ID channel ID, right, maybe I got some you even have you even have mentioned in here ,and then you do you you do the logic of ,if this is a message for a particular user, like for this client here ,and it has mention in it, then this client will now update you know the channel that has the mentioned, to show you one mentioned ,where is there already mentioned you booted up the slack app and there were like 5 mention and we'll just increments it to 1 on the client



and of course if we have this is a nice thing about the system is like you got a notification up here, [to the pub sub system is that means that a client, first wrote to are persistent storage ,]{.mark}which means that if you if you if client here like received a notification, and then just refresh the page, they will not have the updated information in the persistent storage and so everything will kind of be as it should be does that make sense



I'm just trying to think we just walk through, like one edge case, imagine we're both on slack, I come on I don't have any unread messages, you send me a message as I get the notification, to show that I have an unread message ,was saying like our one-on-one chat and then if I don't open that message, and don't Mark as read ,and I refresh what I just said happens ,like you will have written to the database and I will have my last Channel activity will be updated to have a very recent message with my last read receipt for that channel will be older than that, so I'll have new read receipts, if I do look at the at the channel like when I get the notification, then all write to the database with the mark As read and if I refreshed then it'll be marked as read ,and the beauty of this is that also because there's a network issue ,and we get no pup's up notification twice ,it's effectively idempotent and effectively doesn't have like it doesn't impact not going to make me have an unread Channel twice, I won't be able to Market channel as read and then get it unread again ,because everything will also be in the persistence solution



okay I think I think that covers it all is there anything else that I'm missing I think that's all I wanted to hear I'm so unless you have other questions for me with that we hope that you found this video informative and we'll see you in the next one
