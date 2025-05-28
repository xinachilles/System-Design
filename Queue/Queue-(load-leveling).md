# Queue (load leveling) 

Created: 2019-10-29 22:18:07 -0600

Modified: 2021-02-11 18:00:18 -0600

---

<https://channel9.msdn.com/Series/onacloud/Queue-Basic-Overview>



Use a queue that acts as a buffer between a task and a service that it invokes in order to smooth intermittent heavy loads.





~~the typical the application show be something like this. you have web service where client making request to get web pages like jason from API.~~



~~then behind data store traditionally it like sql database more and more it becoming to no SQL.~~



~~whatever is you actually have a data store which is persist data say like user data, product information depend on what industry you are working on.~~



~~a typical web request coming in, you have web application on the web service, they call data store. it's good to add small or moderate scale. it's ready start having issue with world wide web as we know today.~~



the first thing we do a lot of time when we are looking the simple system like this. we are introduce the queue. It's basic a same application request being made it but when it's doing writes of the data. it will stick into queue and that point later, it will be written to the database. (But you typical gets, we still come from database)



[it really able to control over the actually write to the data store]{.mark}.

It is typically a bottom neck specially on the cloud would be the data store. it's supper easy to add more web application node so stand up more web services. it's more ticker to create scale data architecture.



insert into queue than insert into database in later point



2

<https://channel9.msdn.com/Series/onacloud/Queue-Load-Leveling-Pattern>



basic we see intent is [spiky traffic]{.mark}. ~~so you can pick whatever time frame you want here~~. ~~what you see is~~ at certain point, you have very high volume of traffic coming to you site VS other time you have near 0 or very low amount compared to what you see at the high end.



what's that mean to you web service or data cue you need handle the spikes as they coming.

So typically we are talking about architecture. ~~we are talking about over provisioning.~~ you are provisioning service or resource to meet the maximum potential scaling you need.

The reason why it is greate.



on a tire like database tire. This will become really expensive in the term of like ratio of your entire solution. you entire solution need premium price point just due to the spikes.



But by using queue in the middle. we call load leveling. so basic the data flow is coming to you system or inserting into the queue and later on inserting into the database. the load leveling part of that is

we cannot dictate the frequency of message adding to the queue. but we can dictate rate of the message are dequeen and insert into database



we take very spiky traffic then we load level it. we can average that out over time to meet kind of middle bar of your traffic. so we are taking off all those peeks. we can take off all those valleys. we can do steady stream insert update on your database.

if the peek is 100 transaction /s

and valley is 1 transaction /s



At load level, we can coverage out the 50 transaction /s . we just need worried about the database can handle 50 transaction/s. it don't need worried about the 100; obviously

anything below 50 would be safe.



















![](../media/Queue-Queue-(load-leveling)-image1.png){width="0.24305555555555555in" height="0.5in"}

