# Algo expert



---

Solution to this question in this interview I want you to design the algo expert website



At first thought Diego expedite a website seems like it covers a lot of different things there's the homepage, the purchase page ,the questions list, the code execution engine, are we designing the entire platform or just a particular part of the platform like the code execution engine for example





I want you to only worry about the core user flow on Sonos thing, about the payment system or a certification, you can just assume that these work out of the box supposed to be also because we use third-party systems like stripe and Oauth





Core meaning a user goes on the website they probably see the homepage and they going to questions list they can click on, questions or mark them as completed ,and then type the code and run code on the platform



If we take that as the main system or part of the system that we're designing, the first thing that I want to try to better understand is the type of availability that we're looking for, in this system my guess is that I'll go expert isn't it was super critical life-and-death type of system we probably don't need to shoot for the highest type of availability that being said ,of course it is a paid products I'm assuming there is some expectation of availability ,I'm thinking that we can probably shoot for somewhere between 2 and 3 nines of availability does that make sense



where does that get us in terms of that like down times per year, can you can you give me like a number of hours or days every year that we would kind of have as a buffer



sure so I'm thinking that since this is a paid product , we do want some level of availability ~~this is that's what I was trying to get across there,~~ so I'm thinking if we're going for two to three nines of availability probably anywhere between three days of downtime at most, but if you know at least you ate 8 hours of downtime because I'm thinking we might have issues or relying on third-party systems like stripe you said ,or what was it oh ah there's also I think the videos which were not going to be hosting ourselves were probably going to be using video I think our YouTube



so we're relying on third-party something anywhere between 8 hours and 3 days at most is that acceptable or should we be aiming for something better



I think that's reasonable



this the kind of framework them operating under, mainly means that I probably won't be making any extreme optimizations in favor of availability in a suit for high availability, but that's just what I'm keeping in mind there



The other couple of characteristics of assistant that I think are obviously to be important and may be particularly important for a algo expert are latency and throughput I suppose, that for most of the website latency and throughput actually aren't going to be particularly important ,we're just going to expect in a normal relatively fast website ,but for the code execution engine which is very interactive users are actually running code and they probably have an expectation of speed or they want to see their code ,in the output of their code really fast ,we're really going to care about latency there. so does that make sense that that we would that we would want to focus for latency especially on the code execution engine



how fast do on the code execution engine to be?



I'm thinking that if I were user on algo expert would expect a reasonably fast run code kind of functionality, probably I would I know that it would be done on the internet so I don't expect it to be understand but almost in anywhere between I'm thinking 0.5 seconds to the 4 seconds, let's just say we're going to we're going to shoot for 1 -3 seconds of latency for the Run code and we'll see if that kind of falls apart later on when we actually get into the implementation of the design



(okay yeah let's do that okay so I guess we'll all right some of these things down I'll write that we are shooting for you two to three nines of availability and I'll say that we're shooting for your normal latency for the website and I normally just me and you for the Run code and actually going to quantify that and say 1 -3 seconds for the Run code)



and then I guess what type of scale are we expecting for this website ? how many customers are we building this for? How many users at a given point this time?



so I'll go website receives about a few hundred thousands of users every month and you should be able scale you're designed to supports thousands of users at the same time,



okay so 1000 of users basically at any point on the website so maybe tens of thousands of users at any point in time, so this is going to be the significant current users



last question that comes to mind before we can probably dumped into some high-level design, some high-level actual design is are we serving only customers in the US or we serving a global audience, serving specific regions



so that's a great question we want the website to feel responsive to people everywhere in the world ,and the US and India are the top two markets for us.



So we have a global customer base, but we're specifically like probably we specifically want to optimize for US and India as regions, I think I'll be probably a great design with would taken that into account for sure





US and India are kind of like our primary customer segments where we might want to optimize some stuff, okay so from a high little point of you as far as the design goes they here where my thoughts are at the website is not just a [static website, it has some static components to it like homepage and just the general javascript,]{.mark} the Javascript bundle, but it also has quite a bit of dynamic content purchase the product ,and then once they purchased the product even though we're not going to get into the purchasing the product

service, once they've purchased the product, they have these questions that they can actually like mutate or rather they can they can save that on the website,



right we want we want users to be able to save their Solutions you said , so that means that we're definitely going to need a robust API behind the website, to kind of power the website ,and we're going to need a database to back that API because we're going to be storing data not only the static data but also data pertaining to users like their Solutions their code Solutions I guess their completion status of questions, we said



so what I'm thinking the way that I'm thinking of dividing this design or rather this this plan of attack of the design is first dealing with static UI content, the static content for the user interface, then maybe dealing with the sum of the axillary services that we have like Auth and payments which I guess I'll probably skip over that stuff ,but that would be my natural second step is axillary Services, then maybe dealing with the core user action is related to seeing questions, typing code seeing their own solutions to code or to questions and then finally the fourth part would be the actual running code on the website like the code execution, I guess is what we can call it does that make sense has the four kind of steps



I will start with a UI static content and I guess, I'll write them down probably as I go, but you guys had a Content than axillary Services which I'll skip because we don't worry about payments and Auth, then core user actions and then actual running code, so for the UI static content, this is stuff like the homepage the JavaScript bundle like I said, all of the images that are on the website ,either they're quite a few icon on the website, generally spreaking, we can expect to have image on websites.



what I'm thinking for that as we can we can [store static content in a blob store ,like you know Google Cloud Storage or Amazon S3]{.mark}, ~~so I'm actually going to write that down up here I'll stay for UI static content we are going to store in wall just right blob the store I guess blob store and here I'll put GCS for Google Cloud Storage with a blob store that I'm familiar with or Amazon S3~~ and



since we said that we really care about US customers North America customers as well as India customers what I'm thinking is we might want to use a CDN for the static content actually serve the static content, because the homepage is important for the homepage of the website you want people to go on it and have it be very responsive ,some thinking that a CDN like Google Cloud CDN or another product I'm familiar with or and we can use a cloudflare one of those CDNS server the static content and that would be good , because basically [like the CDN was would have its own servers in India or near India and the US, and we would serve static content ,really fast and that would probably know reduce the response time]{.mark} of the homepage of the purchase page quite significantly for all users regardless of their region



~~I'll actually write this down so he said it's stored in a blob store and then it'll be a we'll have a CDN, CDN to serve ,it CDN to serve and hero put a Google Cloud GCDBand will be Google Cloud CDN or I guess cloudflare Cloud~~



~~so this is what I'm thinking for the static content and again just to repeat one last time this is going to be Java script Bundle, any assets like images that kind of stuff, I think that's probably as far as I need to go for the UI static content~~



the next part we said we would skip over the axillary service so I'm just going to assume that authentication and payments were taken care of, so I suppose this next part that I'm thinking about which is going to be like the life of a request kind of algoexpert.io l will involve a little bit of auth and payments,



but the first step that I'm thinking of going with now is load balancing right like when a user gets on the website gets served the static content it starts hitting actual algo expert API, get served the static content from the CDN ,but then they actually Hit algo expert api.





Yeah I like we going to want some kind of load balancing and since we said that we cared about the Region's so much like US and india, I'm thinking that a DNS load balancer right off the bat ,would be would be a good approach, so basically like whatever user is depending on their region ,we're going to have a DNS load balance reroute that request that users request to a cluster servers that users given region that we would probably have two or three major hubs like one in North America ,one in India and one in maybe Europe , if we think Europe is unimportant a reason





what I'll do then is an actually going to draw something out, so I'll have a couple of circles, these are going to be in a different clients ,so for instance the top one might be a Us customer or us client, in the bottom one might be an India client, and these two clients are going to Issue request to algoexpert.io, there's going to be for the DNS load balancing that happens and then depending on their region, their requests are going to be your routed to two different clusters of servers ,so I'll route one up here and one down here, ~~as opposed here like it doesn't matter which one is us which one is in the other both essentially equivalent, but what matters is that they are different in different regions ,and~~



what I'm thinking is that once the requests have been load balance or distributed rather according to region, we should have another layer of load balancing based onthe types of requests that they are, because if you take a step back and you look at algoexpert.io, there are a lot of seemingly isolated services ~~and you can correct a few things this is a bad Direction~~



The code execution engine for example is very different from the payment service, it seems like it's something that should not influence each other, so I'm basically thinking of doing some kind of path-based load balancing at the regional level depending on the type of API call



that you're making so let's assume that the algoexpert.io api looks something like this like / API / or / payments or / your code run code execution or whatever depending on this path at the region level here ,there will be another a set of load balancers for another router set of load balancing that that redirects or re-distributes these requests



you think they're that make sense what's do that so then here



I will add a few you know sets of load balancers that basically I do this path-based on load balancing, ~~(I might make myself I might give myself some more space in a little bit but for now let's just continue~~) and I suppose that even the path-based load balancing ,we can probably assume that here are like multiple servers per path or per backend service with multiple servers handling Auth and multiple service handle payment , we can do futher load balance, like round-robin load balancing for those servers, but I'll just assume that that's almost like captured in the path-based load balancing



~~so once we have that I guess now I'll probably right right ear or else I'll keep the yellow color but all right arrows that points were application servers~~ we've got a set of application servers that are here and these application servers are going to be basically in a doing the logic ,like oh you're you're making a request to make a payment ,they contact the third-party payment service ,making a request to run code, they contact whatever we're going to design a little in a little bit ,one thing that I'm thinking is, for certain parts of the platform or for certain API requests ,there is an access control point of view, like it's you if you own a logo expert, if you bought the product vs if you haven't bought the product the server should handle things differently, like it could return a different set of questions or lie questions where you don't see the solutions of the videos yet ,and so on and so forth,



so we can do pretty simple Access Control implementation here which would be just the application servers make some [internal API call to see if the user has permission, or to at see if the user has access to the product]{.mark} and this might be some sort of like tag on the user account ,for something that's handle authentication and payment services



~~those 2 service would probably give the user the permission, but so these back and servers as I'll just put an arrow for now they point to like some other service , which is like the ACL service that just says like another internal API call~~



once we have that what would we move on to next, I guess here we would start to talk about the actual what I called the user flow of you know accessing questions, I probably starting with the questions list, and then individual questions list.



so are you comfortable if we moved to that?



~~yeah let's do that let's do that so I'm going to move the screen a tiny bit here~~ when you think about the data on ioexpert.io we have there really two types of data, there's is static data again ,because the questions list on a logo expert is static, it doesn't really change the way you write problem, the question titles and all that, but then there's also user data like my Solutions, as a user whether or not the question is completed ,~~by the way are we doing are we handling things like systems expert or like the data structures course~~



interview: no let's when you think about what kind of data we want store, just think about the coding questions, and to simplify things forget about user written tests even just think about the so the static content our solutions for every language, the completion status for user for every question forget about the ordering of questions and then for each user has has their own solutions for every language and so how to say that too



~~okay okay so that's that simplifies a little bit I think that even if we did have the other stuff that would probably be very similar, but so I guess I stand by what I said,~~ of having like a distinction between static content versus use actual user content like their solution ,and so she already thinking that we can have two different types of storage, then we will we can probably store are static content ,meaning the actual questions list in a blob store again like GCS or S3 ,so I'll actually read this here I'll put in a database with play, let's put it in blue and this will be like blob store and this has the static questions list, and if a user requests like that the question is less what we would do is, does a request gets routed to application servers, does application servers do the ACL check, and then depending on the ACL check ,they request something from The Blob store, or perhaps here we could have a request something from The Blob for the questions list ,and then they made the ACL check and parse content out with whatever they got from The Blob store as well I don't think the ordering their necessary



but what I am thinking is that we could do some cashing here ,because the questions list let's see the questions list will get access to a lot by a lot of users and I guess it'll get access repeatedly by users, in one session, a user might go on the questions list three or four times ,they're doing multiple questions or they just exploring the website, so I'm thinking we could actually have two layers of cashing to avoid having to make a bunch of requests to our blobstore, so we will have one or you know few request request to our blob store, but I'm thinking that we could do maybe at the client level right here ,so literally in the browser, in our front end code we could do some cashing [some client-side caching of the questions list ,so when a client gets the questions list.]{.mark} they actually cached the questions list ~~so I'll put like a yellow. Here to show that work cashing you know client-side caching of question list~~



then I'm also thinking that at the server level of the application server level right here, we could also cash the questions list, because the client site caching would be better because a single client doesn't ask do multiple requests in one session and that means the single client has a better experience on the website

cause don't experience any latency the second or third time, they were requesting questions. and also here, we have multiple people are requesting the questions list which is definitely gonna happen , our server doesn't need to keep going to the Blob store, so I'm actually going to put some server-side caching here, we can just like cache in memory right here, for the questions list



does that make sense yeah,but is cashing all the data are reasonable? and also we might be updating questions every like two days, but we kind of want the any change to that static questions list to be reflected within the hour for sure?so how much did are we talking about and it is the strategy actually reasonable given our refresh rate?



for the refresh rate, we can definitely cash eviction policy and I'll get into that in a second ,but first look and see if this is even reasonable ,so estimating 85 questions ,for the sake of simplicity ,now they're going to round it up, I'm going to do this more you liberally around this up to a 100 questions, with an upper bound on how much data we were going to be passing or we might be passing ,what's a 100 questions, the question titles are trivial compared to the actual solutions that were saving in the prompt, let's assume that we have 7 solutions for every language ,like a user saves like write Code for every language they also have the prom so that's like 8 big pieces of text, we said we don't care about the tests, but even if we did that would double it, so let's just round this up to ten, 10 big blobs of test, and say that a solution on average has I don't know anywhere between 1000 and 10,000 characters or bytes of data that go with 5,000, 5000 bytes of data per language, multiply that by ten that's 50,000 bytes of data * 100, that is 50000 * 100. that is 5 million ,over 5 million bytes so we're dealing with million so 5 million bytes of data so 5 megabytes of data, 5 megabytes seems reasonable, in in memory to cash a memory here, but this is a an upper bound estimated that someone would write all the languages were expecting of 5000 bytes per language on average ,5 megabytes you comfortable with store this in memories





~~of here all I guess I'll write in memory casting of ql here I guess I misspoke~~ we're not cashing user written Solutions I didn't I didn't over a snake that much were your solution so this is actually think a pretty fair estimates and but I'm still comfortable with the caching ~~of and sorry I realize I'm running out of space~~ then we are gonna be making updates to the questions, you might be adding questions ,you might be fixing bugs that does seem important you would want that reflect its users soon, so I'm thinking we could have a very simple cache eviction policy where we just invalidate and evict data from the cash every like 30 minutes, something like that we just get rid of the five megabytes that are stored and reissue ,the next request will reissue to the Blob store and then come back in cash



I'll just put it here you know 30-minute eviction policy,so I feel pretty comfortable with this but this is only the static content, this is what I just said I corrected myself here we're dealing with a static questions, what's we're not talking yet about the user solutions, like the user created content ,so to speak,so let's actually jumped into that ,we're going to need I think I'm more structured data base for that because a blob storage let's hear we're probably going to be wanting to query this content a lot ,I don't think we're going to go with the relational database for this we can probably go with postgresql for first one user metadata, and I'm going to call this I guess using metadata adjust user created content, I'm thinking that we may want to have two tables to two primary tables, one that would be the question completion status









Want to have we may want to have two tables, two primary tables one that would be the question completion status and one that would be question Solutions, because those are kind of to two separate flows on the platform



does that what does that make sense before I dive into the actual rows and Columns of those tables



the completion status and thinking will be you know, it'll have like a bunch of rows, every Rose is every row is a question, and so it has a user ID, ~~so I'll put u i d I guess it has also you know an ID field for the Axl Rose that cure ID~~ but then we have a user ID, and I'm assuming we would get that from Auth service, then it has I guess a question ID then he would have like a completion status, we are able to put "complete", "in progress","not complete"so basically this would be like an enum string probably and yeah that seems like a reasonable table that means of this table would be like we would have every user would have 85 rows



interview : it looks like for every user for every question, you're going to have a row and given the fact that people can Mark questions as complete or not even you know if they don't purchase,

we might have many many of these row, table looks like it could get really really large, so you think it would be fast to get the completion status of the user for everyone of us questions





I guess I think you're right here, it might be a little bit slow to sequentially go through the entire table, so what I'm thinking is we could we could index this table on user ID, so ~~we could do this would be~~ what we're writing to this database is going to be just when I use remarks at question is complete or not and that's something that we don't we don't really care too much about the increased duration of the write to the index



so I'm thinking we can we can add an index on the user ID for this table ,totally fix the performance issues



we're going to have a second table which is going to be for the solutions, so this table would be I guess again like the rows would be per question again, we would have an ID field ,we would have a user ID field ,we would have a question ID field, and then we have language, because right so we would have any language field, so here I think on this table we would actually have 85 * 7 is there 7 languages



that says 785 x 7 rows per User, it's almost like somewhere between 600 and 700 rows, I think we've been similar to this like you suggested having an index on the user ID, and then we can we can Index this table on user ID ,and on question ID ,because we're going to be going on individual questions a bunch ,



interview: it does it does and so I assume that it in your design you basically be respond like giving the solutions for specific question and a specific user about for all languages back to the UI the same time ,how does that change if we start adding you know if I like 20 languages instead





I guess if you yeah I saved the language has become too large ,you may not want to do that so what you might do is you might I mean if you only have twenties ,still doesn't seem like it's sequence iterator through, you might add an index even on the language of that points , also keep in mind that we can we can do some client-side caching for this as well, so that it's a user because it's the same question a couple times. It's already cashed,but I guess that doesn't really matter where we're certainly not going to be doing server-side caching because users don't share any data, like it sounds like static content where they do share the data, so they don't start but yeah I think we could add an index on the language if we think that were at a point where we have a lot of languages and it warns having an index





I think this is it for the data model as far as like actual you're making the write requests to the database ,obviously are our application servers are going to be making these requests ,so they're going to go to the database and this is not going to happen every time, that a user either checks off a question as complete or make it as in progress or whatever

they're typing code





so I do believe that we would want to debounce on the UI when you're writing code to not issue write request every 0.5 seconds or whatever we will probably have like a 2-3 debounce and I think that that seems reasonable like if we have we said that we expect to have like what's a 10,000 users on the platform at once ,for ten thousand users let's say that 80% of them are running a typiping code on the platform ,so 8000 of them and they're all typing at any given point in time you might expect that we would have more like an in one second, we might have a thousand requests happening if we have a 2-3 section D bounce and a thousand write requests per second seems very reasonable for database like postgres





what do you think yeah but does that story change with the regions



~~that we have dealing with separate region, so I could write that's alright this says I'll write it in in white here ,this is India for example and this is us or North America maybe and you know India might serve southeast Asia or something, and we might have another cluster for Europe ,and I think it would have multiple database servers, so here there would be another database server and the servers will be going here~~



and when thinking is I want it honest on any particular database server I think we can handle the amount of write that I was that I was talking about a second ago, but do we want these database servers to be in-sync ,because the reason we have multiple database servers here is because we want this database server to be close to India, so that India customers have low latency ,and same for this for us ,but users like a u.s. customer will never access an India customers data, so US customer should never hit this database unless of course the u.s. customer like travels to India



so I'm thinking that these two databases like we need to have some kind of a synchronous replication, but it doesn't have to be synchronize, it doesn't have to happen immediately set to clarify for u.s. customer is making a write request to this database, we will make that request, and that's it maybe we'll have some redundancy here to make sure that his database goes down ,we have a backup stand ,by his far as like our main database clusters here like Regional cluster, I think we could have some kind of asynchronous replication to hear that happens every 6 hours ,we're trying to avoid the case where a u.s. customer has all their data stored in the u.s. database ,they fly to India they happen to go on I'll go at algo expert cause they love the platform and they don't have their data in this database,



that would I guess that would be like really bad it would be a very small edge case but it would be bad reputation every like 6 hours 4 hours that



I think six hours is plenty enough on its way probably won't happen that often and the flights are generally pretty long



yes I think that's my six hours of sleep should be okay yeah and and plus I'm thinking that you know this is something word for optimizing the system afterwards and we have matrix on the system everything we can we can kind of liked tweet this accordingly



but so I'm thinking that we have almost everything here I guess it was the big-ticket item the last big-ticket item is going to be the code execution platform now, which I'm a little scared by because this seems like a daunting task like how much in details you want me to go here



yeah I don't want you to think about this security aspect, because this is it's not really like a distributed systems problem is more like an operating system problem and so I don't want you to worry about that at all



I think how we would handle like the actual request not the actual implementation of running the code or making sure that the code isn't like corrupt or something, ~~so let's imagine that here you know we have our path base load balance saying that's kind of off screen now~~



but this goes to some kind of run code server here, the server is red servers handle the running code and I suppose that one thing that would be important kind of from a security aspect I don't know if you didn't want me to take care of that, but I think we need to rate limit here, because this seems like an obvious place where we would get like rate-limiting or like requests abuse ,at people would might try to bring the platform down by doing denial-of-service attacks, so I'm thinking of just putting some rate-limiting here ,is that okay I think it's fine



how often do you think it's from like a product perspective how often should people be able to run code



you want people to have a good experience, so you probably want them to be able to run Kodi and at least once every second ,like if they want to run code a couple times in a row so once every second or once every 0.5 seconds that would be like the rate-limiting I guess ,more than that and you use throwing error, maybe we would want to do something a bit more complicated like tire-based rate-limiting, so every 3 seconds they can only run code or they can only run code like 3 times every 5 seconds ,once every second and five times every minute cuz it's beyond five times in a minute seems like something weird is going on



does that make sense yeah yeah more than five times for a minute and they're likely like scripting something





basically the idea would be rate liming here I think that you know we can use a simple like key Value Store, like redis to implement this rate limiting



that the first layer of deference then we are not taking care of the actual security related to the code, like you just said as far as handling the traffic, what I'm thinking is that these servers ,application servers that are actually handling the request, we don't want to actually have them be the ones running code, [we want to have some other service running code,]{.mark} so I'm going to wait here a couple of circles that represent, let's calling code executes centers or workers ,and basically our application servers here ,application service here or are sending request to these workers for run the code, and these are the ones actually running the code and you know I'm assuming that we would have some sort of you wait for these servers to know when a worker is like in progress, or when a worker is resetting itself, we probably don't want these workers to reboot because I would take a lot of time and remember this is where the latency comes into play, we care a lot about the 1 -3 seconds wait-and-see aspect of the code execution engine ,but so I'm thinking that these workers will still have a point in time where they have to either they finish running code and I have to clean up zombie processes , extra generated files and applications servers would have to know, not to send another request another worker ,that's in the process of cleaning itself



[maybe there would be some sort of like queue based system]{.mark} for the point is we would have these workers that would be handling the Run code separately ,and yeah I did say the 1 -3 seconds was important for latency I feel like some languages like JavaScript and python would be really fast, you don't have to compile them, java or C plus plus might be slower cuz you do have to compile them ,I'm singing still on average is this part of the process this here, could be like two seconds on average and then you know you have like the additional requests overhead here, and we we do reach that 3 seconds bound and of course we can optimize this



so I'm feeling kind of comfortable with this very loose design here is that make sense



that's perfect how many how many of those workers like expect to need given the number of people like on the website it at all points in time ?



so we said10,000 people on the website



I'm like I was my loose estimation earlier based off of your tens of thousands in a day ,then your 8000 on the code, like actually typing code at a time ,and then run code you don't run code that frequently right, you probably run totally 5-10 times and an hour-long session of writing code ,so I feel like at any given point in time, I am you probably have you know 8000 / I don't know like 10 or a 100 if you're running code that's that's infrequently, I'm thinking you have like the 10-run code request per second anywhere between 10 and a hundred and something that you know you have the equivalent number of a worker machine ,so you might have 10 to 20 to a hundred worker machines at any given point in time and this feels like this is a part of our system that can scale horizontally, very easily what you just add workers, if you if you see that your traffic is higher ,we also scaled vertically by making it be giving them more resources like more CPU ,and I'm assuming that it by the way that these workers we can have a machine or machines rather with 8 core like MacBook Pros here, running code



~~exactly something like a TV use the actual like compilation time should probably closer to 2 like a second or 2nd and a half at most~~ s~~o so yeah that estimates pretty correct~~



so I guess I'll just right for clarification here that like 10 to a hundred workers depending on like you know depending on the on the traffic, that you're kind of expecting you know and let's put it this way horizontally and vertically or horizontally scalable,



~~not sure if there two L's to horizontally but we'll go with two L's so yeah~~



I think that that's kind of what I'm thinking for the system because if I have to go back to the beginning here I think we've covered most of our bases I will say that just as a bonus, I know we're running out of time it is a bonus like we would probably want to have some pretty robust logging and monitoring in place here especially for the code execution engine, if we wanted to keep track of how many users are running coded us at a time and what language I, hear I'm thinking that form for workers with my optimize this basically based on or logging and monitoring and whatever the data tells us we might ,even separate these workers out based on language is we're out or forward request to specific language workers, this would all come you know as our system grows and logging and monitoring can help with that so that's kind of like a bonus, that I'm thing if we want to really optimize this system ,but otherwise like I think I like I'm feeling pretty comfortable with this system, you have any any other things that you'd like me to cover or not I think we're pretty much at a time but yeah your your your point about the long and my ring is is is is really good that's probably how we would we would auto-scale number workers ideally, so we don't have to do it manually ,would that I hope that you found this video informative and we'll see you in the next one

![](../media/Onsite-quesion-Algo-expert-image1.png)![](../media/Onsite-quesion-Algo-expert-image2.png)


