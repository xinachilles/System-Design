# circular break 2

Created: 2019-10-30 23:13:04 -0600

Modified: 2019-12-12 12:56:35 -0600

---

[Cloud Patterns for Resiliency (Circuit Breakers and Throttling)](https://www.youtube.com/watch?v=yVnVY2HPVsI&t=482s)

It is going to be failure on the cloud. How deal with that.



circular break help us protect latency and concurrency. Latency mean sth take to long. it same as failure.

if take 5 minutes save work item, we are done.

So we need protection latency.



concurrency : for the database , there are limit for how many simultaneously connect you can connect to SQL database. if you go beyond that, problem may happy.



How to prevent issue just due to too many call once



in the old day before we have circular breaks, database may have problem, we are queueing up a bunch of call of an aps.net ,when we fix the problems, then what happen next, boo, here comes whole wave of calls. it's got to deal with so many simultaneous calls

goes down again. The circulars repeats. The whole point of circular break is shut load quickly



Limited how many staff can be pending to the system and order allow it to recovery



the key of circular break is if you are going to failure do you have way fall back and gracefully degrade



how's circular beak work

as the call coming in, it go to circular break, normally circular break is closed. The normally things are following through. its looking at the failure rates, when the failure rates exceed some percentage and give certain windows of time with certain volume. it's gona say something horribly happen. the circular break will open





what is open. it just start failing calls.

It is by the way about blunt instrument so you may have problem in the code and that problem factor may have in affect been triggered by somebody's behavior but we start to fail all the call the save the system. Circular break is all about saving the system. To prevent the system from going down. To prevent Frof failure from spreading throug rest of it



thing coming through. the thing start to fail and when realize it open. it start fail all the call

if i fail all the call, how do i know the window reclosed.







circular break actually occasionally let something through as a test. you may have 1000 call a second come through instead when it is open, you might let 10 go through became you try to figurate out it heath or not



if the resource is some dependency of ours , the other things that good about the circuit breaker opening is we take pressure off that system



we need feed a little bit through to find out it is working because some point we need reclose go back to normal

the status call half open, we just it's kind of flooding a little bit through





.






