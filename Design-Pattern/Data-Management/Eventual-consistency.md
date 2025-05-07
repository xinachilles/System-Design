# Eventual consistency

Created: 2019-12-05 19:29:45 -0600

Modified: 2019-12-06 10:46:47 -0600

---

There's a lot of benefits to it typically when you're talking about eventual consistency or systems that are Loosely coupled on a Loosely coupled systems are typically much better at keeping up time also can make it more easily to scale the right areas of your application.



I'm just going to go back to my always a consistent with application talking to a database so you know historically you've had like your server, your VM whatever this is talking to a database.



The idea being that you know when a call is made to what they were doing like a save. I'm still going to save a piece of data that actually goes to the to the web server . HTTP traffic is put to the SQL Server.



~~Transaction is run. So the actual life . Consistency of the data throughout this whole thing because there's a transaction in place. It is means that this is really pretty much guaranteed at [this point that this was like written to disk]{.mark} . So it is very tightly couple.~~







There's a lot of problems and scale with this because you know if you have a lot of saves happening at the same time maybe your database can't keep up when you actually start erroring these out which it which might not be a good behavior for your clients



So this is really you know tightly coupled the web server will not be successful unless the database is successful



That's kind of tight coupling there a way that I would look at it now just going into a really quick simple loose coupling





The idea here is that time to take whatever was being saved you put it on a Q and then later you'll stick it in the database



so from the client's perspective it kind of changes greatly so the Save action has on the web server, Webster puts it on a Q and then that the coupling is over. There's no fully fully understanding of if that item hit the disc or not



But you know as a system developer you should be able to say you know what kind of guarantee this(save to the queue) and we guarantee this (save to the disk) these are too kind of separate guarantees that make



this is kind of now becoming loose couple where you're saying that you know if it makes it to the Q. I trust that we're going to put it in the database right so basically you're going to say to your client was save. It is the same as actually eventual. So it's not yet happened and the way.



The latency seemed to really small will you be talking millisecond delay in receiving enough 10 seconds minutes hours whatever you choose a on the latency.



so now I'm kind of a bigger one now let's try and talk through it so we're going to do like Let's pretend We're Facebook and we're going to do like. The idea that there's a piece of content on a web page and you want someone to hit like



So you'll have your like being like posted to the server. Server will be running. In one case I was actually sticking it in like a nosql cable and so if you got a piece of content and it was like five times with in like a certain time frame. We would just keep adding it to the no SQL at table column data and so all of that we just keep getting pend





and then there was a serious break in terms of this wouldn't trigger any action on the system this would just be like a running process let's just say like every one minute. It would look for new batches. So this is just a repeating process that has really nothing to do with like it like.



~~This you know there could be more thoughtful around that it had a little bit more of a push I just want to be really basic here as to what's going on~~



and so every minute then the database is getting the count so in this case it's going to pull the four it's going to do the math and it's going to store 4 likes to this piece of content into the database.



So giving a process like like where it hits your web server you stick it in like a nosql approach and then a background process is just slowly Gathering that data and pushing it into like a database



~~you can see here this is really like truthfully eventual consistency the idea being here that this almost has no impact as to when that will hit the database. This is really the governing Factor as to when this with the database do you have a really good control over when you want to process and how much you want to proceed all of that kind of stuff right~~



and again is true in the middle layer where you know your dequeue. You can choose that writes.



This is eventual consistency and as a system architect, the idea think about really is how long do you want this to take. Until the faster you want to to to be, the the worse, it's the kind of the worse it's going to scale, right or harder to be the scale.



if you can make these numbers in like the minute time frame this is extremely easy to support and extremely easy to support at scale



~~This gives you a lot of options and breaks her system down into kind more simple components that are doing simple thing. I really I really like that on from usually cost perspective but definitely performs and scalability perspective~~



so really recommend start thinking about moving things into eventual consistency they don't work for everything like on a user sign-up definitely a portion of it you wanted to be kind of his transactional.



or if you're doing like monetary things they typically will fall into this transactional model.



but if you're doing things like catalytics, elemetry, aggregation of data. It is much better breakdown. Slow but just guarantee that you're going to deal with in within a specific time frame


