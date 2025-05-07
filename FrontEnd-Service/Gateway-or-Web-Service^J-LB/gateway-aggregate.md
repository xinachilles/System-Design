# gateway aggregate

Created: 2019-11-11 10:48:05 -0600

Modified: 2019-12-16 10:30:27 -0600

---

<https://www.youtube.com/watch?v=fZkMxA_TKS4&t=2s>





let's say that we've got this application right here which handles benefits, eligibility and determination across the state or county that you're in our government that you're in and so anybody can apply for these kind of benefits.



In this works well with microservices because each benefit: Child Care, Dental, utility Health all of these are separate and independent of each other and therefore it makes really good model for microservices



[There's a 140 of these benefits right here and so is this works well until we get this kind of request : what benefits is Mark receiving.]{.mark}



[oh you say well just orchestrate that there's a hundred and forty of those]{.mark}

[I would have to make 140 separate restful calls in order to find out.]{.mark}



does Mark have childcare ? does Mark have dental health?



[You might say well no don't be silly just run all those in parallel . oh so we're going to make 140 parallel restful calls using an orchestrator and the problem their occur is what's the probability of any one of those timing out . It because I certainly can't just continue to pull and return what I can.I need the answer kind of right away]{.mark}



[Here's the other problem each of those different microservices all 140 of them all have a separate schema then if that schema is stored in the same database.The same physical database]{.mark}



[That one single request is going to consume 140 database connections and so you can kind of see orchestration won't work for these kind of request]{.mark}



so how do you do these and that's something called an aggregator



[we would add another microservices separately deployed micro service.]{.mark}

[Called benefit aggregator.]{.mark}



[This is going to do a store that aggregated information]{.mark}



[now it gets that information is that we have queue and we have something called a data pump.so all the child care for example Dental a utility how 140 of them. Every time I get a benefit or I get a benefit denied or removed it's going to pump that information out to the Queue.]{.mark}



[now the benefit aggregator in turn reads from that date pump and then correspondingly updates or inserts that particular information.]{.mark}



Now here's the question : Are we duplicating information? the answer is yes however out of the thousands of attributes across all 140. [All these have is two pieces of information: the name and the benefit. Mark has child care if Mark has utility and that's what these aggregators are used for. Their used to accumulate or to reduce certain information to be able to handle that request.]{.mark}



Now with this in mind here's the new one point from the API layer that says get all or it what benefits does Mark have.





Now I'm able to now per request these are also dedicated to specific requests spend 17 milliseconds to go out to my data to a query and end up returning that information now these benefit are these aggregators right here



[Another great use case in that is for cross-cutting concerns as well. Let's say that there's a new rule that says no individual can have more than 20 benefits.]{.mark}



If I got a child care to say goodnight is Mark eligible for child care well?Now a new cross-cutting rules as well he can't have more than 20 benefits. How many does he have? [I can go to that aggregator because I also have that same information this might be a case where I'm I overload that aggregator with a how many does Mark have .Because that's just the select count]{.mark}





I can use in inter service communication through either rest or messaging for child care to go over to the aggregator and say by the way how many benefits does Mark have. Because I don't know I don't have that information the aggregator does and it simply returns that information back to childcare.



[You might be concerned about data synchronization and data loss and there's a couple of ways of being able to handle that. Within the data pump the first way we handle that is something called a space synchronous.]{.mark}



[When I send to a persisted queue I wait until I get the acknowledgement from that broker that it was able to persist that message. so that was first thing long with persisted queue's.]{.mark}



[The aggregator would use something called a client acknowledge mode where Once the data pump receiving a message, data pump don't acknowledge that I received that message to the broker until it actually done data pump commit in that data. Once I've done that commits then and only then do I acknowledge the receipt of that message]{.mark}



There's another way you can actually synchronizing that something called the checksum pattern and with this pattern I can stream out the information from Child Care, Dental, utility Health all140 saying I am sending information and here's my service ID and request ID.



The aggregator correspondingly streams out information maybe the Kafka saying I've received these and then we can do a Reconciliation. Given time to make sure that the messages that were sent were received. And of course that there's any pending inside that Queue


