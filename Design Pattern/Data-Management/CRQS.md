# CRQS



---

command query responsibility segregation

<https://www.youtube.com/watch?v=pUGvXUBfvEE&t=194s>



it's really about segregation command query

it's start the program first. we have typical program application we have web interface on the right had side and going through application all the way to the database on the backend.



~~you can see the "bottom path " are all the write to the database and the top path all the read to the database~~



we have couple of problem play almost every software application. First with the model. because write and read have different concerns. so other words, usually we do write to the database, we write the entries. but when we do the reads, we read the aggregate date.



Second problem, which is more import, it is the affect we cannot optimize for read and writes. in other words, if we want to speed up the reads from a database. what i can do is adding index and index to the table, which will gone to significant to increase the performance of those reads or those query. however, our writes or our commands will significant slow down. it will start time out, it will start get complaint from users about updates delete, insert those kind of commands



so what we usually do with response is straight to remove indexes in order to speed up the writes which slow down the reads. basically we always end up this compromise. a great way to find compromise. it's affect well everybody agrees but no one really truly happy. So we not optimize for reads no writes. it is kind of balance this happy path.



~~the first thing CQRS does is said let made first have separate model for query and separate model for commands. the commands mode is typical entity based mode where the query mode is more for aggregation and data grid.~~



~~in term of the command is going to be entity and query be the aggregate kind of object someone call presentation object.~~



~~in the middle tire really take those entire from the database which are objet aggregate them into different model call a presentation object and sent those to UI. this is first stage. but it doesn't optimize the database~~



so the second part of CQRS is really saying let's optimize the database now to where we split the database into a read database and write database



in a write database is coming into a fully optimized database for writes in other words no indexes whatsoever in that database and the read path coming back is fully optimized for reads which means that there are indexes everywhere in that table and so now we have maximum performance for both reads and writes and what we do is we have database synchronization from the right over to the read database a great pattern of architecture.



However the problem is on the left-hand side here and it's synchronizing that data this is usually too slow and it's also error-prone and so this is what's usually plagued CQRS



~~but as we observe here what if we took the application that has that query in the command and actually split that into two separately deployed units of software and if we start to look and say well that's an interesting thought as a matter of fact each of those separately deployed units of software has its own single purpose so we're still using that single responsibility principle and each has its own data and you know what that sounds like that sounds like micro server~~

~~and so let's now see how we can actually apply CQRS to microservices because what we have on this far left-hand side is a customer demographics service now let me~~ ask you this what is the model in that micro service optimized for is it optimized for reads or writes.



you might think well hmm actually that's a good point and we have to have kind of two models within this micro service one to update entities and two to return an

aggregation of these and so that's one problem



[but more so what's that database optimized for and neither reads nor writes and so what we can do is this we can take that customer demographic service and we split it into two services. We split it into a 1. customer demographics query service and this contains name and address and bill-to and ship-to addresses and payment information and now we can actually have 2. an update service as well now rather than trying to synchronize.]{.mark}



~~those two databases which we can try to do a very typical implementation here. By the way oh and I might back up. it's not only the model in the database it's the scalability as well. How many reads do we have. we have 300,000 reads how many writes do we have about 3,000 a day and you can kind of see that there is a huge difference in scalability needs in terms of the small amount of writes to the massive amount of reads and so this is a good use case for splitting these two~~

~~services up~~





but what about the data so each one has its own model but typically we get rid of the database on the query service (service 1 ) and what we do in the update service or the write service is that we expose a cache we have a cache of all of

our data and we [replicate or distribute]{.mark} that cache to the query service (service 1 ) so that when an update does occur in the right service the update service now we synchronize that cache through either replication or distribution and



~~this could be either like apache ignite it could be jem fire coherence hazel cast main cast d all of these kind of caching tools or it could be simple messaging updating a map inside our query~~

~~servus now what we have is two services~~



~~that are fully optimized for query and update which now we have a single source of record and now we can scale these and optimize each of these for maximum performance and this is a great example of how to leverage CQRS~~

~~in a micro services ecosystem as a~~

~~matter of fact if you want to get more~~














