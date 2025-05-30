# stock 



---

I would like you to design a stockbroker, stockbroker is a platform that acts as the intermediary between M customers and some Central Stock Exchange form



~~Is it similar to the Robinhood phone application, or the Etrade website~~



[what is this platform supposed to support]{.mark} exactly, I'm assuming that we're going to be supporting buying and selling stocks, but are we supporting trading other security like options and Futures ,are we supporting special types of order is like limit orders or stop losses what exactly are we supporting here



right so we're going to keep it a little bit more simple than that we're I want you to support only [Market orders on stocks]{.mark} in the sign, in a market order means that given a place order with our brokerage platform ,we should try to execute it with the exchange as soon as possible ,regardless of the stock price ,we're also not designing any margin system ,so the available balance is the source of Truth for that customer for what can be bought





I see so let me write that down we are designing a system that supports Market orders only ,for Market orders for stocks, that's the main just that I think



and are we designing any sort of auxiliary services that some of these platforms support like a depositing funds or withdrawing funds ,downloading tax documents that kind of stuff ?



No, we just design core trading aspect of the platform



Just Market orders for stocks for trading aspect of the platform



are we are we supporting other types of operations that might be related to two orders like getting the status of orders or your listing all orders ,



so we could do that if we we can do that for an extra time but I just want you to design a system around a place trade API call from the user and I would like you to define the API call ,so what kind of inputs it has and what kind of responses ,we can expect from it



okay so actually specify this then we are working with a place trade API call, you said this is going to be the primary API call that ,we're going to be making, but if we're not if we're not supporting functionality ,you like withdrawing funds ,depositing funds like, how are we getting a customer's money to actually execute orders, will we have to interact with a bank directly ?



a no so that's another part of that you won't be interacting directly with the bank, you can assume that our customers already deposited funds into your platform and you can assume that there is a SQL table ,specifically SQL table with the balance each customer who wants to make a trade



write this down as well, we have a SQL table for customer balances, is that fair to say okay so we have a SQL table for customer balances ,am I going to be writing down to SQL table like I'm I going to be coming up with its schema?



yes, I'd like you to come up with a reasonable kind of schema for that table ,



[how many customers are we building this for?]{.mark} s[o I suppose like is this a global audience?]{.mark}



so I want this to be able to scale to millions of customers ,and millions of Trades a day, we can assume that our customers are only located in a single region what's the u.s.





So we said millions of customers , you said millions of Trades per day , so it is going to be a very very heavy system and you said only in one reason like the u.s,



let's just make it available only for a first single region only



~~I suppose this is design would probably be the same as they were for another region in the world to the point is we're only catering to 1 region~~



~~what time does looking for the system~~



[what kind of availability are we looking for this system.]{.mark}



we do want high availability with this kind of service because since it's dealing with money and stock, people can lose a lot of money really quickly , even it down for a few minutes



so I write it down, high availability for this is platform, probably something I wanna optimize I will keep in mind ,so high availability availability needed



~~I guess let me see if there any other things I'm thinking about here, since we're designing designing this place trade API call most likely ,do we also have to design a UI for this platform ,or what type of clients are we are we expecting to be to be interacting with this API~~



~~you don't have to design the UI but you shouldn't design what kind of API call it will be making to the backend that you're designing, and for the clients you want to think of him as either a mobile app or a web system~~



so we really concerned with API and we assume that will have you some sort of UI, like a mobile application consuming our API, that's what you're saying,



and then I guess my final question has to do with at the very beginning you mentioned that this is this platform as an intermediary between end users and some Central exchange



so my guess is that the I am designing this "place trade API" is actually going to be interacting with some other API that the "exchange" provides is this correct? and also like it is that is correct what does this exchange API look like do we have any guarantees from it





so you're right you're platforms API, " place trade API" will eventually have to interact with the exchanges API ,and as far as that is concerned ,you can assume that the quantity exchanged to make the trade will taking a call back. with in addition to the information about the trade that your are asking to execute and that call back will get executed one that trade completes at the exchange level ,meaning either when the trade gets filled or rejected, you can also assume that the exchange has a highly-available API, so that your call back will always get executed at least once



okay I see so you're saying that we will definitely be interacting with the exchanges API and you said that we're going to have a call back that we can pass to it, it is guaranteed to be executed when the exchange completes a trade is that correct



yes so you can think about it as you will be notified by The Exchange when status of a trade that you have made changes



~~gacha so I'll read this and yellow again since it has to do with the API ,we will have a call back that we can pass the exchange call back ,so we can pass up with this is an arrow to the exchange that the exchanges API ,and this will be kind of a on complete call back, I'm assuming that we're going to want to have some pretty important logic in this call back that we might want to execute basically, asked we have that that notification from The Exchange that that we completed a trade ,but I guess this might become a bit more clear as we continue on with the design of the system~~



I think that I have most of what I need here, I'll just reiterate the system that were designing just a really makes sure that that I'm not missing anything



[we want to design a stockbroker this is going to be a platform that sits in between and users and an exchange]{.mark}, it is the end users ~~are interacting with it just like they~~ would interact with a website or phone application, and they're going to be placing trades, use trade going to be done through the single API call called Place trade, they're going to Market orders meaning that we you said the market orders means that we execute them regardless of price, like at the current price right





then we said that we have a sql table for customer balances and we're serving millions of customers in the US are in a single region, millions of Trades per day, that's a lot of trades , I want to high available, highly available system and we had this exchange that's highly available itself, gives us this call back that's guaranteed to call when they try to complete



[okay the way I am thinking of approaching this design or kind of like splitting it out so to speak, is in 3 or 4 steps of categories,]{.mark} guess the first one would be to really figure out what to hash out this place trade API, like in a see what it looks like, when you said you wanted me to to define it with parameters, response, type and all that



yep I want you to define a signature of the inputs and outputs of that call



I think that would be the first thing I can do, the second thing would be design from a high level at least, just [what are main backend servers are going to look, like the servers that are going to be handling all of the place trade API called]{.mark}



then finally I think that we can jump into the design for the actual the part of the system that actually handles trades and executes trade, and this will likely be related to The Exchange ,this will likely be the part of the system that communicates with the exchange



does that make sense from a very high-level



okay okay so I will give myself a bit of space, I guess I'll just write it at the top here very quickly, the first we're going to do the API call , then the second is going to be our API servers, API servers, and then we are going to do our actual, let's say a trade execution system



for the API call we're just designing this place trade API, I think that if this is like what you see what happens when a customer goes on the app, on the website, and buy the stock for instance ,they they have to pick the actual stock, so this API calls likely going to take you to the name of the stock, stock ticker, they have to define whether or not they want to buy or sell the stock, cuz it could sell the stock so you will have like a type of the of the order of the trade order, there is likely to be a quantity ,right that people can buy and sell multiple stocks is,



I'll write it in red is going to be something like you know place trade ,and it'll taken before he takes an all this parameters that I just mentioned the stock ticker, the type of the order the quantity of of stock ,I think it'll also have to take a customer ID ,because you are making a trade on behalf of a customer, that here customer id ,then I think this will be stock ticker stock ticker, then it'll be the type and the type like I just said this is going to be like buy or sell, and then there's going to be no quantity and ,by the way here ,~~I know that we're not designing limit orders rather types of orders, but I suppose we could have ,if we were we probably would have another parameter here that would define the type of the order, and depending on that if you have even more parameters but for now~~ I guess we want Also to clarify the customer ID this is really something ,that'll be generated kind of automatically, you know by the client based on authentication tokens, or something like that before he really done securely ,dude you want me to go more into detail about that or



response for this is because you want me to find a response at this place trade API call





the response will probably even though we're not going to be designing getting trade status or my order status is a list of statuses,



I'm assuming that we would still want some kind of identification for the order, respond with order ID, ~~so I'll write this down here you'll be kind of like an order ID~~ this would be generated by the back end, and some way and then I think it would just respond with probably you know this the same, the same parameters, hear it reconfirm basically the stock ticker, the types of quantity, stock ticker, type, quantity



then I think since we're dealing with a response. You always want a few more things probably want the status of the order, cuz maybe the order got rejected immediately ,or maybe the order



although I feel like since we are going to be dealing with the exchange ,we're probably the exchange ,we said we said issue an API call to this exchange to know to actually executed trade, and it's only when we get the call back, it's only when to call back that we past to exchange, when that call that gets executed, only then do we know that our trade kind of like either succeeded or failed,



I said that I think we still probably would want a status here ,so I'll write a status, but I think that this status, ~~I think this will always be just like waste use place as the word for your~~ us put an order that has been sent successfully captured, but it hasn't necessarily been executed or filled or it hasn't necessarily been rejected either ,it's just been placed,



this will be the de facto response, but maybe you'll have other stuff do you have other potential statuses later on, and then finally we also need we also need like the time that this was created at, so I'll add this with a created at ,created at this is going to be I guess a timestamp for when this order created , when does order rather this trade was created ,



is this what good for our for API call



I think this is great can you talk to me a little bit about the actual servers that will be handling this API call



let's see we are going to be we're going to have a lot of servers rather be able to have service they're going to handling a lot of these Place trade API calls, that's that's the thing that we know for sure, and we know that some at least some of these, actually all these of these place trade API calls are going to also affect the customer's balance ,cuz if you buy a stock you have to deduct funds from your customers balance, if you sell a stock that the increase the funds ,and you specifically told us that we have an SQL table for a customer balances ,right so we're going to definitely be editing this SQL table ,at some point in the whole flow ,probably ask for the call back gets executed on complete ,so maybe not immediately but we will eventually be be dealing with, this this kind of tells me that we probably want to also store all of our trades, are all of our orders in a SQL table ,especially since like I know once again we're designing this very simply ,with just the PLace trace API , but he does feel like this platform would eventually want to support your listing the trade, listing orders, I think that having all of our trades, are all of our orders stored in their own SQL table ,would make sense



does that make sense to you, or yeah let's do that can you tell me a bit more about the how these tables look like,





[I'll try both the SQL table for the trades and the SQL table for the balances, ~~so~~]{.mark} ~~the SQL table for the trades let's see all all draw a bunch of columns for now and maybe I'll eliminate some in a second l~~et's assume it's going to be a large table or white table it certainly going to be this will certainly be a very large table as far as the number of rows goes ,because we're going to [Spring]{.mark} all the trades from all the customers in this table ,



we're going to want every trade to have an ID ,and I suppose this would be the the [trade ID]{.mark}, or the order ID that we would return here ,~~in this some I'll write this down here~~ this would be automatically generated ,and this will probably be an integer or a randomly-generated string, then we're going to want to store the [customer ID]{.mark} ,cuz we need to know who who issued or who initiated this trade, so I put CID here, then we are going to want to store the stock ticker, we're going to want us where the types, I think we're getting one is for like all of these parameters that we passed here, so I'll put SC fo[r stock ticker]{.mark} ,I'll put [Type for buy or sell]{.mark}, I'll put a quantityfor [quantity]{.mark} ,we're also going to store the [status]{.mark}



by the way I only mentioned "placed" earlier, but I think status will be placed, it'll be rejected if the order has been rejected for whatever reason, it'll be filled if you order got filled, and there might be I guess I can in progress while we while we wait for the exchange like in between the time that we exchanged and between the time that we are waiting for the call back to get executed



I just said I said if it's rejected for any [reason]{.mark} just makes me think that we might actually have multiple reasons for rejection, are you could imagine that you might get in the order that gets rejected because you didn't have enough funds, you might get an order that gets rejected because you know you're outside Market hours or something weird ,like that so I'm thinking that we may want also another call in for like a reason, like a human readable message that explains why something wild trade got rejected , Or Filled for filled probably that wouldn't be relevant ,but for Rejection it seems relevant





do you think that makes sense yeah yeah if you could also maybe I guess you would add it to the response in the API, you don't need to



okay this is this is the this is a final parameter on the response,





I guess we should also draw the balances table so the balances table will probably look relatively similar, I guess by the way, we will you will likely want Index this table on one or more of these columns right, now I'm thinking it'll likely be like maybe it'll be the type of of order, or something like a [customer ID]{.mark}, cuz you probably you know you'll probably want to get all the customers trades at one point, but I'll table that for now and we'll see we'll see a little bit later



but so for the balances let's see the balances it'll be all so ID and customer ID, ~~I think we might not need that many problems but will definitely need ID will definitely need customer ID and assuming we'll need like oh I guess I forgot in the~~ trades table here I forgot to put the created at Y~~a this this parameter will definitely need in our in our columns here so real~~. Created this is just a SQL timestamp,



~~Van Atkins but so then here customer ID for the balance~~ for the balance there isn't that much right there's an amount to the amount is for dealing only with the US, this is floating Point numbers that represent US Dollars, and then is there anything else I guess we can have maybe like a last modified field, show up late like last_M,would be like a timestamp for when we last modified the balance, but beyond that I don't see anything that would be super relevant to us right now



we could if we wanted to we could add a currency column, but for this~~,~~this particular purpose were only serving us, and we can just assume everything's in dollars so that's fine



now we're at this point where we have these tables rather with all the call ones and I am happy with what they look like, but I'm trying to figure out like how exactly this is going to get us actually executing a trade



I guess the idea is we are talking about the service , that are API servers that handle millions of place trade ,a API calls a day so far ,if our API servers are are here for instance ~~what's right then let's ride them here~~ these are API server as you know we were probably, going to have a lot of them, [they are getting calls from our client ,clients that are making these calls]{.mark}, [I guess these that we would have some kind of load balancer, in between ,we would definitely want to load balancer to distribute or cluster load balance to distribute are these, red arrows here represents the "trade place API" call we want to this load balance distribute the load across our servers are main API servers ,]{.mark}



now the main question that I'm wondering is do we want any type of like super special server selection strategy at our load balancer level? let's see what we're just placing trades this is a very very taxing operation in the sense that there are a lot of them, with but itself it's not super taxing, and [we don't really need like any type of cashing for this, I don't think we need any type of like server stickiness here,]{.mark} so I don't think we need to do [we don't need to optimize the server selection strategy for the load balancing to to make sure that in a certain clients calls gets forwarded to a specific server setting, we can just do like round-robin]{.mark} here round-robin load balancing ,does that seem



okay to show that work doing round robin round robin load balancing ,and then we have our API servers we have also these two SQL tables ,what do we do with them so we want to store our trades in the trade stable ,this seems kind of obvious so you know we would have our servers make write to this trade table, but then we want to actually send these orders to The Exchange ,now the problem is if we are dealing with, [let's say buy orders we need it's actually very important ,the order in which we execute the trades]{.mark}, and it's very important that we not get below, like that our customer has enough money right, and we can only know, if the customer has enough money from The Exchange ,because a market order you said gets executed at the current price regardless of the price right



yeah okay so basically what I'm getting at is we cannot eagerly respond to the client and we cannot eagerly update the balances table ,or like mark the trade has filed or something, before we have talked to [The Exchange, and very important that we talk to The Exchange in the order,]{.mark} that trades come in ,either the the earliest trade, the first trade that came in hit in our servers , or the oldest trade rather is the one from a from a specific customers the first one that should be executed



~~I'm a little bit worry of having all of our servers, I guess these servers aren't the ones ,these servers wouldn't be the ones, who would be talking to the exchange ,like if we had our exchange here, write it in in blue, this is the exchange ,~~we probably wouldn't want these servers to be directly interacting with this exchange ,~~like they're already handling a bunch of load hear,~~ something that we would like another set of servers like maybe workers or what I call workers here, that would be handling you know all of the trades, maybe grabbing them from the SQL table and and themselves going to The Exchange and waiting for responses ,and then maybe updating the SQL table ,but I'm a little bit I'm a little bit worry of this design, because I feel like it is going to be things are going to go very out of hand ,if we have like a ton of SQL queries happening in the table at once ,whether they're coming from workers here, or from from the servers, if they're all trying to update like the same customer, especially since we went with a round-robin load balancing approach ,do you have like two different servers with trades from the same customer, trying to make an update to this table or something,



so what I'm what I'm trying to think of if we might ~~not~~ be better off using like a a messaging assistant ,like a [publish subscribe messaging]{.mark} system ,where we would we would basically route customers or customers trades, the trades for pertaining to specific customer ,[we were to Route them to a messaging service ,special topic or special channel for that customer]{.mark}, or for a set of customers that was the same customer ,would always be routed to that to that topic, and we would have then or other set of servers are workers subscribing to those topics ,subscribing to those channels, watching for new trades or notification for messages coming in grabbing them off of these kind of message queues and then talking to the exchange





~~that way I feel like it would be easier to reason about, a bit cleaner than than trying to have like servers all is suing queries to his database, all at once or whatever customer, and I think we might be able to handle this just in a little bit more properly, does this make sense I know I'm kind of all over the place here~~



interview: so you're trying to use that messaging system to ensure that for single customer only one machine will be executing or or trying to play straight with the exchange at any point in time



the idea would be, lets say we have a the [cluster of workers]{.mark} ,and by the way the reason [we have we might have a cluster rather than only single one would be if we want to really optimized for that high availability,]{.mark} and I'll get into what I have in mind the second ,but you have like a cluster of workers ,that would only be handling trades or not not necessarily they would be handling trades for multiple customers, but [a single customer would only get it trades handled by a specific cluster workers.]{.mark}



So the follow from start to finish would be like this,



this is an end-user, I make an API call, the "Place trade API call" which would be because I want to buy a stock, it goes to the load balancer, or the set of load balancers, it gets rerouted to one of our API services, round-robin here ,we don't really care about [service stickiness,]{.mark} [this server makes a write to the trade database, stores the trade in here,]{.mark} but on top of that create ~~so I'll write this down I guess you know stores it in this database but on top of that~~ it creates a notification for a message, pub/sub message, by the way here we can use Kafka,or could pub sub and google pub/sub





we would have these topics hear,that would be Pub sub topics, that would be that, would have clusters of workers subscribing ,maybe I'll write them in in blue, here would having a topic number one topic number to topic number three ,in here alright another clusters of workers here these are separated, right these are different clusters, these would be subscribing to this, so they would be kind of waiting here , these would be waiting for this and these will be waiting for this



[the API service would create a notification or message]{.mark} that says, hey I got a trade from this customer, let's call this customer C1, I've got to trade from this client, I'm going to hash the clients username for instance, and based on that all, I will route this trade, this message for that trade to one of these topics ,so here we would care about the stickiness of the topic so to speak as, so we would hash it, so let's say C1 would go here ,we would have liked a message here pertaining to C1



we ~~would have C1 that this server pushed right the server would would~~ basically push C1 to this topic, and then hear this would get you know sent or streamed to kind of to the subscribers to the consumers of this topic, and all trades from C1 would go here all trades from what they C2 would go to one of these topics .they would always go to the same one [we would also be guaranteed at least once delivery through these messaging system]{.mark} so we would know that we would never lose a specific trade



The workers from here, they would see a notification and they would contact the exchange, and then we can get into the logic about, like what happens when they contacted. I think this is where a call back we're going to play, these workers would contact you about exchange



make sense for from a from a high level



can you go into how workers within a group ,within a cluster cop coordinate to figure out who does what



so here this is where I'm thinking that, this is where our system might start to fall apart ,like we definitely don't want these workers to fail here ,cuz this is highly available we get some guarantees with Pub Sub, but we definitely and here you know if one of our server dies our load balancer reroute to another server, our SQL database a safe, the cluster of worker is what we really care about, [so here I think we could use a leader election in between or within server cluster or worker cluster,]{.mark} to have a single worker at any given point in time do the leader ,~~like maybe this one here would be the leader may be here we would have this one of the leaders,~~ you would have this one be the leader ,the leader is the one that that takes care of messages coming in the topic, and that takes care of the logic of contacting exchange Three workers to start maybe, five workers would make sense ,and we would have to monitor our system to see if this is the appropriate,



How many different partitions are topics you would have, how many different clusters of machines are to handle the load that we mentioned earlier,



so like [how many of these clusters ,are how many of these topics would have]{.mark} ,probably mirror the number of clusters cuz each cluster subscribes the one topic, but as far as how many of these, will let's see, what was what we said that we are going to be expecting millions of customers ,but I guess the number of customers ,number of Trades per day so do I get to the customers does matters were routing to the customer specific topic, but trade is really the important part ,and if we have millions of Trades per day, how many seconds in a day there are 3600 seconds in a day what's round us up to 4,000 so 4000 * 24 that's roughly 25 what's 100000 seconds in a day, so hundred thousand seconds and if we have a million trades per day ,a million divided by 100000. Is roughly 10 trade per second ,let me write that down where we are expecting roughly 10 trades per second although this is a this is a stockbroker, according my experience, with stockbrokers is that like if you're at least if you're in the US, you only you can only trade during trading hours ,is that correct



just assumed that for for the sake of this is the example



it is not 24/7 system, so actually are our trading load happens during is like bunched up during a part of the day ,would say that it's been bunched up during 1/3 of the day, we just multiply that by 3, so we are 30 trades per second,



and if we like if we don't have 24/7 system, it's more like eight hours a day since then I don't know exactly how much it would be but this would be more like [30 trades per second]{.mark}, the idea would be probably has 30 topics with 30-plus ,maybe you go a little bit above that, so that you have so that the Clusters are handling less than one trade a second, so maybe you would you go with like 3 times this amount, so like [a hundred cluster in a hundred topic]{.mark}



I just wanted to get an order of magnitude I'll just put pubsub and I'll put a hundred topic and again [the number of clusters is equal to the number of topics]{.mark}, by the way just to make sure that we're on the same page, this is horizontally scalable, and also the workers are vertically scalable, although your gets this doesn't really matter. this is more if we see that a lot of our workers are dying, cuz we will have only one leader at once taking care of the load ,so you know but I guess still we can have more than three or 30 clusters ,more than 30 topics ,and then at the end of the day we are dependent on the exchange and how fast it can respond



okay so the next thing talk about I guess is what the actual logic add exchange level, what is happening here that allows everything else to work ,so we at this point when we are here right we have we have stored a trade in our table ,and we have put its status as what we have put its status of placed, of here we have put its status as placed, this is what we said earlier,at this point the order is placed ,we've created a pub sub message here ,that got routed to topic or workers here or workers were the leader of our workers ,what is going to do if it's going to say, given the next message in my topic ,what am I going to do let's see so the message would have a customer [what the message have a trade ID, I don't actually think that the message needs a trade ID]{.mark}, I think the message can only be a customer,



and what happens is the worker of the leader here, that's basically the [~~most~~ the least recent trade]{.mark} from this particular customer by querying the database and because we know that no other cluster is handling this customer, this is kind of safe query to make in our database of this point in time, and we we get the latest or the [least recent trade from this customer]{.mark} and depending on the status of that trade, we go to the exchange or we do nothing, ~~in other words at the trade is placed and we go to The Exchange is the trade of the least recent trade is not and~~ I guess you're know we would be good we would be [looking specifically for least recent trades that are not filled or rejected,]{.mark} right we would be looking for for trades that are that are either placed or I suppose even in progress, you like the trades that are waiting for a response from The Exchange, and base on those we would then you'll go to The Exchange to see what happened to them ,if they're in progress we're just waiting to see if the exchange has a response from them, and if they're placed we actually execute them to the exchange





can you go into more details as to what this SQL query would look like, the one where you try to get the the next trade that you're going to be trying to to check



yes yes so basically said to clarify so here we are we are subscribed to this topic, we get customer 1 we know that work we need to do something pertaining to customer one, basically this tells the workers hate customer one needs attention, so now the worker is going to go up here to the SQL database and ~~let me actually give myself some space so I'll just write the smaller Arrow like this and~~ hear this is the the SQL query that we do you

select *

from trades

where customer id = 'C1'

and status ='placed' or 'processing'

order by created at asc

limit 1



~~basically do select all the star from itradez we are selecting all of the trades from a specific customer from C1 so where CID is equal to see one and this would be this is where remember at the beginning I said that we might want index on customer ID I think this makes sense here~~ we would want index on customer ID so I'll put an index up here, this means that we would want index on customer ID



~~and so we're tid 41 and we want we want what we want we want only trades that are either place or in progress so and status of this is a combination of two things status equals placed and here by the way I'm writing to dudo sql, so to speak status equals in progress so in progress is going to be what we set, when the exchange ,when we like submitted an order to The Exchange, and then since we are getting the latest one we would order by and a limit~~





~~so we would order order by order by what is it created at right this this thing that we had you here who created Act we would limit or order by ascending and we would live in one to get the latest one or Lynette sorry limits one and so this would be our query~~





~~if there's nothing here then ,live in really there's nothing to do in this notification, may have been kind of like at the end of everything I got handles for specific customer, but if there is something if we do have an order or trade, let's go with an order or trades would been placed,~~ then we would issue a request to The Exchange, well first of all I guess we would mark this as in progress, and if we if we receive a trade that is placed ,[we want and I'll make it in progress]{.mark} ,because we're back to talk to The Exchange, so if it's placed we would mark it as progress, ~~so I guess like this would now become in progress, I'm overlapping here ,cuz I don't really have that much space~~





in progress and I'll talk in a second about what would happen, if there's an issue here ,with our Network ,cuz you could imagine that we might set a traders in progress, but then fail or network requested exchange which would be really bad ,like we never wanted to hang right like this type of system, we'll talk about that I think we will have to have some kind of axillary service that handles that ,but for now let's just assume that we can make the network call to The Exchange, okay so now we're going to make a call to The Exchange, and here [we're going to pass a call back to the exchange,]{.mark} that is not handle the logic of updating the balance table, updating the trades table, to marking this has failed or rejected, that was a call back with his guaranteed to be called by The Exchange and I think that's done and then at that point [we mark this message has done, right so that he can be as out of the out of the messaging topic]{.mark}



~~okay so I guess we need to get into now what does call back looks like, for the for the in progress here, if we deal with something that's in progress ,kind of think ,I think right now might my got feeling is that we're just going to contact the exchange to see, if there's been anything like, maybe to see if we've actually executed this trade, to make sure this wasn't some kind of error, and if we know that this trade is indeed in progress from The Exchange ,that we just wait and I think that the Callback is eventually going to come in ,and Mark this as no longer in progress~~ ,



right yeah the logic is is roughly it's the same after you if you get a a trade, that's placed and then you mark it is placed I whatever you do next will be the same as if you had just gotten the order as already in progress





yes exactly for the Callback the idea for the call back would be the exchange will execute the call back when the trade completes, and for the trade will complete only if it's failed or if it's rejected we had that guarantee from The Exchange ,so let's go with the example where it's filled, I have a filed that when it's rejected will be very similar minus the update to the balance ,[but if the trade is filled , the call back that the exchange would have to do rather than the exchange will probably issue a request to either one of our workers are probably like some separate server and he would be some separate serve]{.mark}r that does the transaction in the database will actually write this down up here like



I think we would have a separate service of the exchange goes to when when it's issuing the Callback call that goes here,and this server does queries or or write to the database or of the multiple databases in the case of a filled order and he's write would look like this



select *

from trade

where trade id = 'id '





~~I guess I'll skip writing your begin transaction, who would start the transaction we would select the trade so the exchange would have given or argue lyrics over here the idea of the trade or the order that we're dealing with , some who would select all from trades where~~ the ID is equal to that ID to grab their the current id that we're dealing with



first of all any logic becomes a Spur with now we're going to be working to be [like updating the trade to fill ,]{.mark}because this is a filled trade or going to be updating the users balance, we might be out of there updating other stuff about the trade like a reason that it was filled or that kind of stuff, all this stuff we only want to do once if by any chance the trade at this point in time is not in progress, like what's assume the trade is already filled, we want when we don't want to execute you this logic, so we're going to have to have some kind of you if statement, where hey if we if we're dealing with treade that's already been completed just roll back this transaction



does that make sense updating the customers balance , is by nature not idempotent ,so we have to be careful with that that's why we have to make sure test for for safety that the trade is still in progress and has not been filled or rejected



but assuming that it is in progress then we can do an update to the balance for the customer, and we can set the amount to have , let say they were dealing with the buy order, to be equal to negative what you did. The trade amount, then we can update the trades the status of that trade to filled, and then we can update the reason for which it was filled with could probably be the same reason when we're talking about failed order



it would be a rejection of the reason would be different, but so I guess I'll just write pseudo sql here okay the idea would be you update you update balance , right and you have like a trade amount and again here I'm writing up I'm writing pseudocode rather than SQL just for Simplicity ,but you would have a update balance, you would have updates trade status of filled with the trade ID



I guess here you would also need the customer ID for the for the balance, and then finally you would have a update trade reason, I suppose we might also want in the database we might also want in the database something like execution executed at or something like that, which would be the time at which the the trade was filled or the time it was who was rejected, and this would be given by the Exchange



but so this is like the main logic in the in the transaction when you build an order, okay after this you would commit to the transaction, if you were dealing with a rejected order you would just not have this update balance, but I think you would still up trade stauts S to rejected, you would still updated try to reason to whatever the reason it was rejected is and ask for the transaction after you've committed, the transactions would like this year is transaction SQL transaction after you've done this transaction ,[you would not only want to answer the exchange and give it a 200, to let it know that the callbacks done right so like this would be 200 like, I just finished calling your call back, but you would also want notify the pub sub topic]{.mark} , [that this came from]{.mark} , right because I got imagine that you had multiple trades from the same customer coming in ,like one by one, this first trade would get taken by a worker, it would be a place trade, so you would mark it as in progress ,[and you would go to The Exchange then you go to the second trade or you would go to the second message from the same customer]{.mark}, you would re- get all the trades, you would see the oldest trade is still in progress right ,they basically whatever trade to be sent to the exchange would be" placed not in progress" trade ,and so you would skip you wouldn't we were here we said like oh if we're dealing with the trade that's in progress ,[we skip right and so you would need that you would need another message here, like once you once I trade a has completed you would need this server to say hey wait a second, go back to this customer ,we just finished with trade customer, so workers you need to go back to this customer cuz they might have something that still needs attention]{.mark}





~~and basically so here all we mark, here like,~~ [after the commits notify pubsub that this customer might still need attention]{.mark}, like Because this is actually important right, it basically means like this is how this entire function, it's like this part, it's it's basically how like you are sure that a cluster of workers will never miss a trade from the table



[so it's actually you're you're making sure that as soon as a particular trade reaches a terminal state of either failed or rejected, this the pups of topic gets notified to to make sure that the workers will pick up the next trade, if there is an extra that is in placed status]{.mark}





yes exactly exactly and I think that like if we had a little bit more time we could walk through an example or you got in at two back-to-back messages ,[I suppose I just did right and you would see how this works the first message is get put as in progress, it goes to the exchange exchange takes a while to filled it then the worker grab the second message sees that, hey but the least recent trade is in progress ,there to trades with the least recent ones already in progress, so I cannot deal with the second trade so I'm just going to skip for now but you don't want to skip it forever ,so once that first trade finishes, were gets filled by The Exchange ,and and this is server mark that is filled , we need to notify the workers that hey go check if this customer still had other stuff ,and then you would see that older still one more Trader, that's placed and not in progress]{.mark}





there was one more thing that I wanted to say before I think we are running out of time here, but I mentioned something about a network partition here, and I am scared that we might have cases ,where a trade has been placed but has never been sent to the exchange or maybe for whatever reason it's in progress, but the call back from The Exchange got lost like maybe this here , there was a network partitionhear something, not sure exactly what Edge case would cause this, but I'm scared that we might have some trades ,that do remain like: place for in progress but not handled , and that's a really bad if we're dealing with the highly available system if we're dealing with people's money and thinking that we might want to have another axillary service I'm thinking like will write it hear like a [Reaper service]{.mark}





kind of that would periodically and I suppose you would have to happen pretty awesome maybe every like 10 seconds or five seconds of something periodically query, all the trades in the trade table that our place for in progress and check if they're not being handled ,does that make sense, if they're not being handled it would have the got a job of going to The Exchange and handling them or something like





why the very beginning I had mentioned potentially index thing on customer ID which would be useful because of this query and I'm thinking that we definitely will we also use the status here, so I think indexing on the status would also be useful here ,so I'll add an index and we can monitor assistance to see, if this is if this is that useful, so this is the



overall design that I'm thinking of for the system very complex, but I think that accomplishes our goal of a high availability ,making sure that trades never get lost ,that the kids are executed sequentially that we don't have like negative balances, all that kind of all



that stuff what do you think checks all of the the chance that we want to ask for the system so yeah that's what I was looking for thanks with that I hope that you found this video informative and we'll see you in the next one

![](../../media/Payment^JTrade-Stock-stock-image1.png)![](../../media/Payment^JTrade-Stock-stock-image2.png)



![](../../media/Payment^JTrade-Stock-stock-image3.png)



![](../../media/Payment^JTrade-Stock-stock-image4.png)




