[Cloud Patterns for Resiliency (Circuit Breakers and Throttling)](https://www.youtube.com/watch?v=yVnVY2HPVsI&t=482s)

It is going to be a failure in the Cloud. How to deal with that?

Circular break helps us protect latency and concurrency. Latency means somthing take too long. It's the same as failure.

If we take 5 minutes to save a work item, we are done.

So we need protection against latency.

**Concurrency:** for the database, there are limits for how many simultaneously connect you can make to the SQL database. If you go beyond that, the problem may happen.

How to prevent issues just due to too many calls once

In the old day before we have circular breaks, database may have problem, we are queueing up a bunch of call of an aps.net, when we fix the problems, then what happen next, boo, here comes whole wave of calls. It's got to deal with so many simultaneous calls going down again. The circulars repeat. The whole point of a circular break is to shut the load quickly

Limited how many of staff that can be pending in the system and allow it to recover

The key to a circular break is if you are going to fail, do you have a way to fall back and gracefully degrade

**How does the circular beak work?**

As the call comes in, it goes to a circular break, normally circular break is closed. Normally, things follow through. It's looking at the failure rates, when the failure rates exceed some percentage, and gives certain windows of time with certain volume. It's gonna say something horrible happened. The circular break will open

**What is open? It just started failing calls.**

It is by the way about blunt instrument so you may have problem in the code and that problem factor may have in affect been triggered by somebody's behavior but we start to fail all the call the save the system. 

Circular break is all about saving the system. To prevent the system from going down. To prevent Frof failure from spreading through the rest of it

Things are coming through. The thing starts to fail, and when you realize it, it opens. If it starts failing all the calls,

if I fail all calls, how do I know the window has closed?


A circular break actually occasionally lets something through as a test. You may have 1000 calls a second come through. When it is open, you might let 10 go through because you try to figure out it is healthy or not


If the resource is some dependency of ours, the other thing that good about the circuit breaker opening is that we take pressure off that system


we need feed a little bit through to find out it is working because some point we need reclose go back to normal

The status call is half open, we just it's kind of flooding a little bit through





.






