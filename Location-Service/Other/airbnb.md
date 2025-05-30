# airbnb



---

Let's dive into the question I want you to design Airbnb



so like a lot of other consumer service ,type of products Airbnb, has two sides to IT service right there's a renter facing side ,where users can look through properties and book them and then there's a host facing side, are we designing both of these or just one of them, let's talk about both of these sides of the product



[I just write down host + renter]{.mark} and as far as functionality I guess we want to support host being able to create, and maybe delete listings ,and then for renters we want them to be able to browse through listings, or properties book them ,and managed to them afterwards is that correct



yep for host that's correct but let's actually focus on browsing through listings and booking them for renters, we can ignore everything that happens after booking on the renter side



okay so then for a host create plus delete is okay ,but then for renter you're saying we just want brows and delete browse and we said like booking, but not managing afterwards



then for booking is the idea that when a user is browsing a property was safe for a specific date range ,if they actually booked its or start the booking process ,that property will get like locked, so that no one else can look at it



yes so more specifically, multiple users should be allowed to look at the same property ,at the same date range ,and at the same time ,without the issue but once a user books of property ,you should see it reflected that this property is no longer available for the dates if another user tries to book it for the same time



I see but so like let's imagine that we had two users ,who are looking at the exact same property for an overlapping date range, take you know the same a week for example ,and one user presses the book Now button, on your credit card information or that kind of stuff, [do we immediately lock the property for the other user or at least I can see the other user then presses book now]{.mark} so it immediately be locked for some period of time like in 15 minutes or something where it's reserved for that first user? and it's that first user then ask [it precedes with the book ,pays the website then booking or reservation becomes permanent]{.mark}



make sense ,in real life it would be slight differences based on implantation but for the sake of this design I think that's fair





[I'll put 15 mins lock under booking]{.mark}, do we want to design any auxiliary features, like are being able to contact a host or authentication payments that kind of stuff,



Let's looks really just focus on browsing and became the rest can be ignored



okay and just to make sure that we're on the same page for functionality, since it sounds like we're really focusing on the core like booking functionality, is the idea that a user can browse for location or for listings rather based on location, date range, pricing ,property details is that what we want for users, and then on the host side like I said before we just allow them to create and delete listings



yeah but so through this design of purely filter based on location, and date range ,as far as like the listing characteristics are concerned, let's not worry about any of the other stuff like a pricing and property details



okay so then I'll just put like Geo plus date ,for what we care about Geo or the location, is it fair to assume or [can I design the system by assuming that will be store locations using latitude and longitude ,or is it some other like in a way of store location]{.mark}



yeah latitude and longitude works



so then maybe the last couple questions that I have as far as system characteristics ,and I guess even the scale of the system, how big is our user base in a specifically, how many listings do we have, how many users do we have, and then you know like you availability latency and assuming we care about those things, you know since were we want to serve up-to-date information is that all correct



just consider the US operations of Airbnb, serve 50 million users and about a million listings on rounding things, but so that's roughly the scale of it ,and for latency and availability we do care about those ,ideally also we don't want any down time for renters browsing listings



and you confirm that we do care about availability and region and then what was that last point,me don't want any down time specifically browsing list things should be there like very much never down as much as possible



okay then I'll just underline it important okay, so then have all the information I need ,and I first thought right off the bat ,I think we're going to divide this system into two parts, [one part is going to be the host facing side of the system, and one part is going to be the renter facing side]{.mark} ,[I do you think they're going to have some sort of common part in the middle because they're both they're both going to be accessing listings, right released hosts are going to be creating with things and deleting them ,and renters are going to be listing listings and booking them, so they'll have the probably a common storage solution]{.mark} ,but they'll be interacting with it differently, does that make sense



okay so then let me think, yeah I'll probably put I'm going to get myself a lot of space here, I'll probably put the put the renters on the on the right ,here and [I'll put the hosts on the left ,and for the storage unit in the middle]{.mark} hear, what we're probably going to do is, we're going to want a store in a persistent storage all of the listings, as that's like the main entity in Airbnb ,and I think that we can just have like SQL table of listings, or we can have all the information like the latitude longitude, is it fair to assume that if a host creates a listing is this available like indefinitely ,until it's deleted or if it's booked ,and we'll have some of unavailability if it's booked is that okay



we'll have this one table so I guess I'll write it down ,this will be our middle storage solution here will be maybe make it a little bit less big, this will be our SQL database ,SQL and we'll have listings, this will be our main table, I think this is probably the only table we need at least a start, and it probably will have a user's table and other stuff like that but you said not to worry that's auxiliary features, I do think that since we're dealing with Geo usually when you're dealing with Geo when you have this scale with so many potential locations that we took query very fast, and I'm assuming like when you look for listings on Airbnb , you give the location what's a Lake New York and it gives you locations in a vicinity of the New York right



usually for this type of problem I think that you use a quadtree to store these locations in a region, [quad free where is that basically like a huge grid that covers the entire world or the entire US, and that great it is divided or subdivided in in a bunch of grid that have like every time is like 4 grid write a quad tree,]{.mark}



I think I hear what we can do is, we can store all of our listings in this auxiliary structure or storage solutions, quadtree for fast query. Does that make sense,



quick pause I just want to add a little bit of commentary to this design decision here [with a quadtree of storing all of our Airbnb locations in auxiliary quadtree minimize latency]{.mark} when we're going to be seeing all these locations like listing the Airbnb listings so what I want to say about this is that if you actually take a step back and remind yourself of the scale of the system that were designing hear the scale being the US operations of Airbnb with only about a million listings



you realize that this design choice of having an axillary quadtree might be a little bit Overkill ,it might be a little bit over engineering ,our solution to this design because truthfully a million listings or a million locations isn't that big ,a million data entry is something pretty trivial for any modern SQL database especially ,if you remember that these days most databases do come with a built-in the box optimizations to all that to say for this particular system of this particular scale this solution might be a little bit too much ,now the other hand if we had been designing let's say Google Maps where there we could imagine that we would have probably a hundred million locations or even more cuz if you think about it on Google Maps, you got hotels restaurants shops ,gym, schools you got a lot more locations than you would have done something like Airbnb ,so if we had that type of system with fat more locations then maybe having an auxiliary quadtree would be appropriate for that sale for that type of system, other reason that we did end up going with this solution to slightly over engineered, solution here was because we wanted to Showcase a lot of these different systems design concepts in action ,like quadtree like storing quadtree in memory as you'll see in just a few minutes, using leader election to guarantee availability, to also see that in a few minutes ,so that's why we did that and also in theory you could defend this over-engineered design decision by saying that we're building for scale in the future we expect, the system to scale and so we want to have this solution now already , That would be the another way to maybe explain this all that to say when you're in a systems design interview, do keep in mind the scale of the system that you're dealing with and try not to over engineer your Solutions too much ,with that let's continue with this system's design interview question







okay so then they just think cuz right now we have SQL for our persistent storage of our listings table, if we have this auxiliary quad tree that has all of our listings, [and that is going to be meant for fast latency basically, when you list listings ideally we can store this in a memory in some servers ,d]{.mark}own here and I'll put them here and [these are the servers are going to have in memory quad trees of our listings]{.mark} ,[first of all if we go with any memory that might not be great for availability, but we can talk about that in the second, can we even store all of our listings in memory, cuz the in-memory aspect would help with latency]{.mark}



so we said that we had how many we are 1 million, was things so 1 million list things in our SQL table, I'm assuming that when you ,when you list listings, you want to do [I just want to see a list of listing ,so ideally this squad tree gives the renters all the information that they need to display the listings, the list of listings ,another word ideally this doesn't just give ID's of listing and force the renter to go back to the database]{.mark}, because it gives you all the information, so can we set information about a million listings in the squad Tree in memory ,let's see if we've got a million listings, maybe I'll write it down here, 1 million was single single listing



so if we have like a list of listings on the UI, what we want to show ,you would want to show thumbnail, but can be represented by like a link ,an asset link ,we can have a title of a blurb described it,I think we can give like an upper bound of 10 kilobytes event ,that actually kind of seems big but 10 kilobyte upper Bound per listing, so10 kb per listing



if its 10 KB per listing, that gives 10 gigabytes of storage, with definitely quadtree of listings with all the information that we need ,[such that renters never need to interact with this table that can just interact with a quadtree,]{.mark} it'll be faster than the table in memory that's so this will be our quadree, if you'll just represented with one Circle, this will be the quad tree while I guess the only thing is if we know what is in memory like I said before ,we might have availability issues ,so I think what we can do here as we can actually represent [the squad tree with like a ring, like a leader election type of service, where we've got let's go with three for now,]{.mark} is that three servers, Geo indexes or whatever you want to call them that each hold a quad tree or the quad tree of these listings, so you can imagine that when our system like boots up ,these three servers or however many there are here, boot up and creates or generate the quad tree ,so I guess I'll put you in here q ,and one of them is the leaders, I guess they were kind of in harmony, one of them is the leader



let's put this one and then got two followers, and the idea is every 10 minutes ,an hour ,can figure it out the time period later, but on some interval ,these quad trees pin the SQL database and regenerate themselves or re update themselves to have the most up-to-date list, of listings are Quad tree of listings does that make sense



that's great but so what happens if a host creates a new listing, would it take 10 minutes for the listing to appear to the renters?



no, so what we can do here is I guess this is what we can talk about the actual API calls, that hosts are going to the issue ,when a host creates a listing, let's draw out a few hosts here ,when a host creates a listing ,they issue some create listing API call take latitude and longitude and some other parameters, we're going to have some API servers, let's try them and white hear, some API server is here that will I suppose you know these requests would be load balanced from the host to these API server, I don't think we really need any fancy load balancing strategy, or service license strategy, I think we can just go with round-robin



I'll put our load balancer and we'll put our rld then or API servers ,what they'll do is they will first this host ,[makes a request right to create a listing ,this will first go to the SQL table and add the listing in the table, and the listings table]{.mark} ,and then synchronously assuming this succeeds which will be most of the time, [synchronously it'll then go communicate with our GEO index, and specifically the leader ,]{.mark}this is where the leader election principal here, or system kid of works nicely, and [it'll it'll directly update the quad tree in the leader]{.mark}, so I guess the followers would always have as a 10-minute delay or anywhere between whatever the interval is and the time that a host creates a listing ,and then they update themselves with the SQL table, but the leader gets updated automatically



the leader is truly always up-to-date does that make sense



and deleting a listing ,they were dealing with the same thing for deleting a listing you as a call goes to the listings table ,removes it, removes the row or sets, or maybe it's an soft delete, expiration date for the for the listing, and then it goes to the to the leader here and updates, and I ~~guess here this Arrow was I don't know why I drove directly to the API server this Arrowood go to the load balancer and the load balancer would rerouted to the appropriate API server~~ does that make sense



so that takes care of everything on the house side of things ,is trying to figure out if I'm missing something, I guess maybe I can talk about the case, where the leader does die, I give my leader dies ,and we have a host who creates a server or creates a listing, the create listing goes through the API server, it goes in the table updates the leader and then this machine happens to die



which won't happen very often, but let's assume it does, the [follower or one of the followers becomes the new leader, th]{.mark}is follower doesn't have the new listing that was just created and if the host tries to delete it, immediately after for whatever reason, I guess it doesn't matter what it was just ,they would go to the follower just have the leader that's all we're would not have that location or that listing and their Quadtree and just something happens



so of course does a listing table won't be updated that this this ring here with the leader election concept or sub system handles all the hosts side of things pretty well



as far as renters are concerned ,let's think about the functionality that we want to support so we said we care about browsing and browsing and booking browsing, so what does that look like, well you've got the browsing is probably like a list and point, booking is probably like a book and point, where are maybe you can call it reserved listing, if we're talking about the locking mechanism the 15-minute locking mechanism, I guess that if we do have this booking or reservation functionality we also need to get endappoint



as you would presumably click on a listing from the list of listings, and that would fetch you the entire listing, so yeah I think we're going to write and I'll write them down here will will have three endpoints



get, reserved and list, try to think how complicated they are get, seems kind of simple as casue for get, you would be given an ID, listing ID, we could get unique ID, and you would be getting that from the list, the list of listings with will likely interact with our quad trees would be a list of listings that each have a listing ID, and then getting a listing which is the taking in the listing ID going to the SQL database, and while you're done



~~so I guess I'll probably take a write here, you've got a set of API servers and it's got like a renter hear, that renter makes "get" call and that goes to his API servers will probably have some load balance here, in the next so maybe let me undo this I'll put a load balancer here and the renter is supposed to issue a "get" call, load balancer route set to hear and this just goes to the listings table and return~~





Then form for list, things this is the more complicated one ,if you list listings let's see you passing a location latitude longitude ,you pass in a date range, probably other parameters they said we don't care about that for now, what do we say we go to the quad trees, the I guess we communicate with the leader, so the whole point of the reasons for our hosts not transfer act with a SQL table ,when they are getting multiple lists , you go to the quad tree, [you can traverse the quad Street to find the list of listings in the vicinity of your location]{.mark}



by the way if we're dealing with a million listings at most, this quard tree would be very fast to traverse ,right because I get an ideal scenario that quad tree is 4 to power 10 , is greater than million I think, at least close to it, so basically this will be a very fast Quad tree to Traverse



once you have all the listings that are in in the correct vicinity you just return them I said he's got a bunch of API servers here ,I guess another host or renter here rather, this issue a list of quest ,list listings, I guess it just would have to go also to the for the load balancer and see what we can do for the load balancer , if we want to guys want to simplify things we're going to I'm going to have every renter API call ,goes through load balance here



but then this [load balancer is like API path based]{.mark} , like every get request goes to these API servers ,every list request goes to the list servers, and so on and so forth does that sound like reasonable



okay perfect and then I'll just put like path-based lb ,what's past based lb, this goes to the load balance, and the load balancer reroute to the servers ,and then the servers communicate with the leader here, [so the leader and the leader returns of the listings the location]{.mark} that we said



~~if I'm missing anything you said we don't worry about the other parameters like pricing cuz we would probably like to have to do some other filter, if we're looking at parameters like pricing or like a number of bedrooms or something ,you know you did say we do have to worry about date range rights ,yes~~



I would like it if a book and it says listening has been booked ,okay so we know okay so are [quad trees here have all the information of our listings,]{.mark} and we said that we would probably have in our listings table ,here like a booking column with availabilities or maybe it's another table and we query both tables when we create the quadtree, but the point is the quad tree has a list of unavailability for a listing, and those unavailabilities are themselves a list, probably like intervals timestamp intervals, and if we have a date from our renter, the renter give the dates of like or date interval of December 1st to January 1st, we can just do probably a very quick binary search to see if you notice filter out any location or listing rather ,that has an overlapping unavailability, with the day that the renter provides that was a mouthful but does that sounds like fine





like I said our quadtree will have like a pretty small depth, All Things Considered unavailability for bookings will be also like fairly small, and I would expect that even the most popular airbnb's that have very short stays at any given point in time you probably only have like a dozen unavailable intervals, so binary search is almost Overkill but we can totally do it here ,when is this will be this will be very fast, and yeah then we returned it to the API servers I guess,



~~we want we want like this quad tree for you especially the one in the leader has a very high fidelity of relevance for a recent recently updated list ,so we would run in Cache invalidation issues so no cashing ,but since this is such a fast query to go downstairs the quard tree and even filter out the unavailabe time that's totally fine we don't need any cash~~



in here and I guess we will be issuing this request, like pretty often, like not only you know for every renter who's looking through properties ,browsing through properties but also this [will have to be a paginated type of request right,]{.mark} because we don't want to return 1000 proerty, we wanna return probably like 50 at most or 20 or something, so we'll be repeatedly issuing this call ,but I should be fine ,and I guess the handle pagination probably we can use like an offset ,so let's say our imagination, or payed sizes 50 or 20 then or offset would be 20, on the second page so we skip the first 20, in our list of available listings ,or like relevant listings, if we on page number 2, and I'll set of 40 et ectera





now we are left with reserved call, I'll just add another host to represent that or another renter, sorry so if you make a reserved call, what happens, you first I guess you need check the reserve call would happen when you click the book Now button ,like two users can currently looking at the same listing, they sequentially like in order they click the book Now button ,the first one will get the lock ,and the second one is to basically be told by the system hey someone else like is looking at that property or someone else like book that property, check again later or something like that for the time frame assuming they're overlapping



so the first and needs to check if there's a reservation ,a current reservation, for that what time date range and for that location or for that listing ID, cuz this is based on a listing, and then if there isn't one it should create the reservation that has an expiration



okay so let's see how we handle this, I think what [we do is we store reservations in our SQL database ,]{.mark}we store a table reservations, I'll put Reserve here for reservations ,a table, reservations, our renter, when they issue a reserve called, the load balancer probably routes this to a different set of servers, this a reserved servers, and here what they do they first [check the reservation table though they go to the reservation table,]{.mark} and they see is there a current reservation for not only this listing, but for this timeframe, or this date range, is there is throw error or return some message to the client for the renter, if there isn't then you create a reservation, that means that you write to this database table



[a reservation table probably has like a listing ID a timeframe or date range]{.mark} whatever you want to call it ,and [we said it lasts 15 minutes , we can represent that with like an expiration time stamp, right]{.mark} put a timestamp for 15 minutes from the current time, and then I think I should write this then that means that whenever a renter will try to reserve ,they'll go to the table



when you browse you don't want you don't want a listing of available, either because of it beeing booking or because it's being reserved



or is that okay like something that's been reserved in the browsing we don't want people to start browsing and looking into the listing if it's already been booked so we've been do want the quad tree be able to filter out things that weren't available for the time frame that the user's looking at . even the 15 minute locking period



So then I think what we do is yeah I think we will do this [API service here for the reserve call will behave similar to the guy servers for the host, and they'll updates the quad tree]{.mark} so yeah so this this would be an arrow that communicates with the quad free and this would again be synchronous after it's updated the table



just to make sure this is always the primary sources of the truth, and it's the is this transaction happens successfully then you go local leader in the in a quad tree



so let's see [we have unavailable times of unavailable times]{.mark} that we said before that those are like the book things in the listings for [the time frames are date ranges that have been booked]{.mark} ,but you can't just naively add an unavailable time cuz it expires in 15 minutes



right expires is here like it when you hit this you can always check like is the reservation hasn't been deleted or whatever [you just check timestamp read expiration time stamp if you're after that then you know that there's not a reservation]{.mark}



~~but here so~~ maybe what we can do is like those two lists, the list of unavailability on every listing, maybe those as far as they're like in a data representation or more information about them than just an interval



[that's unavailable, there's also an expiration, that way we can we can kind of represent reservation is pretty well though]{.mark}. Expiration time stamp that we have in this table ,we also add on the unavailability, that we put in the quad tree and the updated quad tree and when we do our binary search operation, that I was talking about earlier ,we literally just skip the ones that are that are more



or rather we use those intervals and we check ,we filter out intervals that are past their expiration, no longer valid , they are both are no longer validunavailability therefore they are available, so we just filter them out ,and that should be like a constant time operation in the middle of our binary search



whenever it where it eat unavailability something like that supposed to be dealing with all things considered very few unavailable intervals ,on so this could all be like a very quick operation that really supports our or latency goal for the system the key



the key thing about the quad tree that makes it really nice as it is within each little region right there's going to be so few last things, compared to the entire table of listings, that you haven't sql that a lot of the operation to describing like filtering and checking for unavailability become really fast



yep exactly the speeds things up even more I think that is all that we really need I'm trying to think if I missing anything but I think that we supported everything that we wrote down at the at the beginning right now I think that's that's all we really talked about so you you've pretty much the entire scope of the design so thanks with that we hope that you found this video informative and we'll see you in the next one





![Hosts Load Balancing Round Robin long) DeleteListing(listingld) API Servers API Servers API Servers Listings Geolocation Index EOLL0nER SQL Database Reservations Geolocation LEADER Geolocation Index EOLL%ER Renters GetListing(listingld) API Servers API Servers API Servers Load Balancing API-path-Based API Servers ReserveListing(listingld, dateRange) API Servers API Servers ListListings(lat, long, dateRange) ](../../media/Location-Service-Other-airbnb-image1.png)





![A listing in our quadtree might look something like this: "unavailabilities : [ "range": n expiration • "range": "expiration : null "title": n Listing Title" , "description " : "Listing Description "thumbnailUr1" : "Listing Thumbnail URL" , "id": "Listing ID" ](../../media/Location-Service-Other-airbnb-image2.png)



![](../../media/Location-Service-Other-airbnb-image3.png)



