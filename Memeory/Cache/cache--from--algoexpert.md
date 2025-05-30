# cache  from  algoexpert 



---

writ through cache



two popular types of caches the first is a right through cash a right through cash is a type of cashing system where when you make an edit to a piece of data when you write a piece of data your system will write that piece of data both in the cache and in the main source of Truth at the same time or rather in the same operation



when you write a piece of data ,you system will write to database and cache at

you're going to make a network request of a server and the server is going to overwrite whatever is in the cash, and then it's also going to make the request of the database and overwrite was in the database that way the cache and the database are always in sync so to speak



now the downside of this is that you end up having to still go to the database



so this brings us to the second popular type of cash the right back cache ,the write Back cache



the servers going to update the cash, but only the cache , that's been immediately go back to the client and so now your cash will actually be out of sync with your database, and what'll happen behind the scenes is that your system will asynchronously update the database with the values that are supporting the cash and this can be done in different ways ,maybe it's on certain intervals so maybe every 5 Seconds every 5 minutes every 5 hours who knows or maybe it's going to be following a different type of schedule



maybe it'll be whenever your cache gets filled up and you have to evict stuff out of the cash



that one of the downsides here is that if something ever happens to your cash and you lose the data in the cache for instance before the database has been updated asynchronously and you're going to lose data and that's really bad



if you don't care about consistency if you don't care about the stale of data then you can totally consider cache



final Concept in cashing with his eviction policies, if not have Infinite Space so you can't store an infinite amount of data in a cash but also like we just said sometimes you're going to be less with stale data in your task and you're going to want to get rid of that still.



there's also the least frequently used policy

first in firs out

even just randomly


