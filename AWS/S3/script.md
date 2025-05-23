Good morning, everyone. I'm Susan Calland. Welcome to our latest awesome hour on Amazon. S. 3 tables. Rk. Is going to be presenting today, and I'm going to hand it off to him. Take it away.
RK Vyas
RK Vyas
00:15
Right? Thank you all for joining today's call. I appreciate everyone taking time to learn about the S. 3 tables to store tabular data at scale. My name is Rk. Vias. I'm the senior technical account manager at Aws, I'm out of La and C, and I've been with Aws for almost 3 years in the storage industry for almost 25 years, and happy to talk about the S. 3 tables with you guys.
So here's the agenda for today's call. We will start with the introduction to the S. 3 tables. We'll cover some of the basics on the new capabilities of the S. 3 table, and then dive, dive deeper into how? You know all this work, together with the other components we have, and also go through some of the phenomenas and the features.
Then we'll talk about some of the use cases and the workloads, so you can use the S. 3 cables and then, lastly, we will do a quick recap of everything. When we we went over
feel free to stop me at any time. If something is, you know, you want to dive deeper, we can always dive deeper.
So
when we actually started long back right with the Amazon, s. 3. A lot of you must have used Amazon, s. 3, which is our general purpose. Bucket. That's used for storing, you know, most of our daily data, like videos and photos and anything you could kind of think about. But over a period of time, this has evolved right people need more and more out of the S. 3 bucket so.
as you know, the
things started progressing, we were amazed to see how people are using the Amazon, s. 3. And what we found is, you know, over a period of time. Customers are storing lots and lots of parquet data files and doing analytics out of this S. 3 tables.
Sorry, I should say. S. 3 bucket. I haven't introduced S. 3 tables yet, but it's the it's the storing parquet files in S 3 tables and then doing analytics out of it. So what we, what we saw is this is a lot of tabular data which is stored in the exabytes of parquet format data, and
and people were doing iceberg Apache iceberg on top of this to to get some sense out of this data, keep it in a open table format, storing all this parquet files and trying to do analytics out of this. So what we have seen is the the this extract growth of parquet files and park, if becoming the de facto standard when it comes to storing the tabular data in s. 3 bucket.
So for a long time. When it comes to storing the data sets on the S. 3.
what traditionally people have done is they try to mirror with the infrastructure system. Right? So, creating the file system. So similar. Concept in the S. 3 is creating the the prefixes, and then inside prefix prefixes. You try to maintain everything so that it looks like the folder structure.
But then it comes with its own challenges. So you have to consider data consistency with multiple applications trying to write to the same data sets. There's also a consideration for evolving data scheme.
the data schema, right? So your schema is also evolving. So you have to consider that. And the complexity kind of keeps increasing as your data set keeps growing, and it's going up at the scale.
So this is where we saw the emergence of open table format, which is the Apache iceberg.
and a lot of people were using the Apache iceberg. So it became like a popular choice for a lot of customers. What Apache iceberg does good is it puts a metadata layer which is the robust framework, and that is the basis for
number of iceberg capabilities such as you can do. Transaction support that flexible schema. I was talking about the see, my evolution the time travel in case if you have to go back to like the last transaction and roll back. So that makes it easier to do all these things on the on the Apache iceberg.
But when
customers were doing the Apache icebergs they were running into a lot of different challenges. So as the data grows at the scale, you know, iceberg has its own challenges where you have to kind of maintain the that data sets complexity.
the traffic demands when it comes to data at that scale. Also, as the application continue to grow.
you need to invest into maintenance activities such as compaction. So people who have used the iceberg on S. 3, you may be familiar with the compaction where you have multiple small files, and you kind of, you know, to optimize the query. You have to stand up your own Ec. 2 instances and
run some compaction jobs on top of it, so that you can kind of reduce the size of the file. So when you run the query, it doesn't need to go through like multiple files. It has, you know smaller set of files to to go through just like a optimization technique for the iceberg. The second aspect of managing the tables is
how you could enforce the security policies and go in and surround it. Right? So if you have, if you have the iceberg tables in on the traditional general purpose, s. 3 bucket, you would be 1st doing security as the S. 3 table. Sorry s. 3 bucket level, then prefix level, and then you have to kind of keep in managing. As
things grow, the prefixes you create new prefixes. You have to kind of manage that policies so that your tables and everything is secured within the S. 3 general purpose packet right? And then, lastly, the operational burden. So I talked about the complexion. That's 1 of the thing. The other thing is the snapshots in the iceberg tables.
Anytime you do any current operation which is create updates, it's going to create a new data set file. And the reason why it creates a new data set file is because it can do. You can do a time, travel to the to the older one. So once you have lots and lots of this files. You have to make sure you delete the older ones. So you are not accumulating a lot of files on the inside the S. 3 bucket.
So to address all this challenges. We introduced the S. 3 tables. So this is more like a managed iceberg. So you don't have to worry about all the the challenges I talked about about scaling, about maintenance jobs and everything. S. 3 tables would take care of all those things for you.
so I'll go a little bit more on the S. 3 tables. So
s. 3 table again, it's based on the iceberg. So it it kind of supports that open format. Open table format natively
and then it's a new construct within the S. 3. So if you go into the S. 3 web ui, you will see 3 different types of buckets. Now, one is the general purpose bucket which you guys must have used it. There's also a single zone express zone s. 3 bucket, which is low latency s. 3 bucket, and then this is the 3rd type of S. 3 bucket, you will see which is called S. 3 tables.
Each of the S. 3 table has its own arn, so that means you can assign the resource policy per s. 3 table.
There's also right now the S. 3 table doesn't have the endpoint, but pretty soon I'm pretty sure by end of this month there will be the endpoint also with the S. 3 tables.
you can kind of. I talked about the S 3 tables so you can create the policies resource policy at the table level. Also, you can take the create, the resource policy at the bucket level
and and and it's also going to have the dedicated endpoint moving forward.
So in addition to to the kind of s 3 table I would say, the the
the biggest thing is what Aws has achieved is the optimize the performance.
We also put a lot of this security. So same security control. So you can do security at the table level very easily as compared to what you were doing in the past with your own Diy iceberg solution on the general purpose. S. 3 bucket.
And then the automatic storage cost optimization. So again, if you had iceberg tables running on the general purpose, S. 3 bucket, you were doing a lot of things by yourself, and the challenges associated by doing the compaction, the snapshot and managing is an overhead, so S. 3 tables would take care of all those things, and I'll go into a little bit more details of those as well.
So with the S. 3 tables
you get the 10 x 10 x the performance so transaction per seconds or Tps, this is
this is also kind of you know. I'll talk about how we achieve the 10 x performance with the current limits. We have on the S. 3 bucket versus the new limits on the S. 3 tables.
also with the compaction automatic compaction we have within the S. 3 tables. It will give you 3 x the faster performance on the query side as well. So if you were doing the iceberg on general purpose, S. 3 bucket. And let's say, if you didn't do any compaction, you may get X performance. But now, with the automated compaction within S. 3 table, you get like 3 times that performance.
Well, feel free to stop me at. No, I'm going throwing a lot of information at all of you guys. So let me know if you have any question. I'll take a pause in between.
I'm not working the chat. So if you have any question in the chat, let me know as well.
Susan
Susan
12:05
Rk, I don't see any questions on the chat so far.
RK Vyas
RK Vyas
12:09
Sounds great. Alright, so I'll I'll continue moving. So
s. 3 table is A is a purpose built for the actual tabular data format. Right? So what it means is.
while your application use the iceberg standards to read and write the data. S, 3 can specifically optimize
for for this tabular data format. So it's fully aware of the data which is being stored is for the I, the iceberg analytics workload. So we have optimized everything internally for that open table format within the S. 3 table,
also within the S. 3 table. We have like a namespaces, so that will lay out data effectively which will let your application achieve like a higher transactions per second as compared to, let's say you're doing on a general purpose. S. 3 bucket.
So just to give a little bit point of comparison between you managing iceberg yourself and the Diy solution with the S. 3 table the s. 3. When I say s. 3 tap s. 3 table here. It's the S. 3 general purpose bucket. There is a limit of 5,500 reads per second, and 3,500, writes per second per prefix.
You know this limits kind of kind of goes up as we scale, but that that is the base limit.
But as soon as you go through the S. 3 tables, this limits are gone up to 10 times. So now with the S. 3 tables. Your tps is 55,000 reads, or 35,000 writes, and again, as your kind of workload grows, we scale with it. But your baseline is, you know, much higher, 10 times higher than what the general purpose. S. 3 bucket has.
So the end of the aspect I talked about the compaction and the compaction is like for the performance. So with the iceberg tables, is.
you know, it continues to evolve, data sets will be keep coming. You kind of add, remove data into records into the table which you're storing. It's going to accumulate a lot of changes, and it's going to create a lot of additional files into the table. So as the table scales, this lot of files are accumulated, these are like sometimes very small files.
And if you try to run a query on that small data files, it's going to take a long time. So what we do behind the scene
is something called the compaction, so compaction is nothing. But you know we consolidate a lot of this small files into a larger files, so this reduces the number of requests that application needs to make when going through the data files and trying to make do a query and get you the output of out of it.
so to run compaction previously. Right? As I talked about. If you were doing this in your diy solution in the general purpose bucket, you will stand up the Ec. 2 instances which could be, you know, ideal for a long time, because the compaction is not constantly running
but you know you still have the CC. 2 instances which is doing like, you know, complexion at some certain time interval. Now with the S. 3 tables, you don't have to worry about it, because behind the scene we are aws is doing that complexion for you.
Oscar Hernandez
Oscar Hernandez
16:05
One question sorry, and the convection runs immediately after
every insert, or how is it managed?
RK Vyas
RK Vyas
16:13
It. It doesn't run after every insert it has its own logic. That's a question I have for the product. Our product manager as well, because what I was trying to do is I don't know if Sumja is on the call as well, but what we're trying to do is I'm I'm in the.
Sumaja Kapa
Sumaja Kapa
16:31
Yeah, go ahead. Just ping the Pm team this morning. So it happens in the background. It's not something which happens right away. So it's a background process. It's it's not something which we see.
RK Vyas
RK Vyas
16:45
Yeah. So so we don't. So what we're trying to do is I'll give you a little bit more context. Sumja and I are working on developing a workshop for the S 3 table. So you guys, you know, customers can kind of test it out in our lab. And one of the lab we are kind of developing is the compaction. So customer can see. You know, kind of put lots and lots of data files. And then
we want to show how soon they can see the compaction, and when we configured the compaction there's no configurable parameter on when the compaction would run, so we had to wait for almost an hour to see the actual compaction. So I would say we didn't get the answer on exactly when the what's the logic behind it when it runs the compaction.
But it does on some some logic basis. We'll be happy to kind of get more information. And provide you that.
Chris Voss
Chris Voss
17:39
Is there an slo on the upper bound? For how long compaction takes.
RK Vyas
RK Vyas
17:46
Oh, there is no slo on.
I mean again, it's a good question for a product team. We can check with them. I'm sure they will have. I would say it depends on the data sets right. So how frequently things are being modified on the file, and how many small files are being generated. But that's that's a good question. Sunja, you want to note down that as well.
Sumaja Kapa
Sumaja Kapa
18:14
Yes, okay, that's the question. Actually, I asked the product team also, and I'm just waiting to hear back from them. But yes, we'll we'll get those answers for you.
Once we talk with the product team. Yes.
Oscar Hernandez
Oscar Hernandez
18:28
Okay. Thanks.
RK Vyas
RK Vyas
18:30
That's those are awesome questions. So we we also have. I mean, stable is so new. It came out in reinvent
last year. So we still trying to figure out some of the questions which we don't have the answers. But we'll we'll get back to you on that?
So there's also a blog out there. I put there to kind of a QR. Code. We'll be happy to share the link as well, which talks about the query performance improvement with the compaction. So that's a nice blog to read.
the second thing I talked about is the security controls right? So before. If you had to go and access.
you would give access permission to general purpose. S. 3. Bucket.
Then you will craft some policies which will require to know the physical location of the data and its metadata. So you have to kind of know the prefixes and where you're storing the data to to give some kind of readwrite access to the objects, but with the S. 3 table it becomes much more easier because now you can do security control at more at the table level.
and then you can apply the resource policy to those tables themselves. So you don't have to worry about those prefixes and any of sort, and any of those kind of sorts of things.
and you give the right level of access to each of the table.
There is also a concept of namespace. So
there's S. 3 table bucket, which is like the higher level thing. And inside s. 3 table bucket, you create S 3 tables right? And you can apply the policies at each table level. But let's say you have, you know, groups and groups of tables, and you want to apply the same resource policy to those groups of tables.
Then that's where the namespace comes into picture. So what you do is you create a namespace, and then within that namespace you logically group all these tables together and give it a namespace. And now you can apply the resource policy at the namespace level versus at the table level.
Cost optimization. So I was talking about the snapshot. So anytime you do 3rd operation on the iceberg tables, it's going to be creating a new file, because that gives the iceberg capability to roll back. So you can go move back to the previous transaction right?
So it creates the. In order to do all this rollback, it creates a snapshot so that way it can utilize that snapshot to move back. That's the iceberg property, and that's what we kind of take advantage of to roll back as well, but creates lots and lots of files over a period of time. So now, if you're doing your own general post bucket with iceberg, you will have to
run some kind of a maintenance job to clean up the old snapshot files as they age out, but
with the help of S. 3 tables you don't need to do all the things you just kind of
run aws cli or SDK, and set the specific time, the age when you want this, the snapshot to be expired, and it will take a lot. Take care of deleting the older snapshot as well as the garbage collection of of those files. So it becomes, you know, less less of the files.
It also removes any unreferenced file as well as a part of this garbage removal. So, as you kind of, you know, take care of removing the older files. If there's any unreferenced files.
especially in the iceberg table. For people who are familiar with the iceberg tables. There's a lot of references to the metadata and the file sets, and also there is a concept of manifest files. So there's a lot of unreferenced things when, if you don't do a
a good cleaning, but iceberg so S. 3 tables. Takes care of that for you.
So so far we covered a lot of things on the S. 3 table side. Now we can. I can kind of go over some of the examples on how you could use the S. 3 tables. But I'll take a pause again here, just to see if there's any other questions.
Okay, I'll take that as no questions.
So I'll go a little bit into more into application side of the S. 3 tables right? So
our goal of the S 3 table is more like a storage primitive. So in the sense.
if you put something on the S 3 table, it's a tabular data. You can't do much with it within the S. 3 table itself. But if you want to make sense of the S. 3, what's stored into the S. 3 tables. Then you're going to utilize some other services like Aws, glue to do Etl of that data, and then also use the Emr
Athena redshift quicksight to to kind of run query on it, or do some analysis out of it, using redshift or or look at the dashboards into a quick site, right? So that's kind of one of the application which which many people do when they try. Kind of very high level overview of how S. 3 tables get utilized.
And right now the integration with the aws glue. It's in the preview mode. The biggest advantage, I would say, with the S. 3 table and all the glue integration is
when you create the 1st s. 3 table. It's it's also like for every region, every account.
There's a checkbox which you check or it, or you do the using. Api. It doesn't matter
where you integrate your S 3 table with all of our services. That means it's integrated with the aws glue itself. So
the biggest advantage of that is, you don't have to go into glue and say, Hey, now, discover my S. 3 table, or the other side is when you delete the S 3 tables right? Somebody doesn't need to go to the glue and say, Hey, I deleted this s. 3 table, so remove it from your catalog. Since there's this tight integration between s. 3 table and the glue services.
This becomes like automatic process. So any s. 3 new to S 3 table gets is added. You will see it in blue catalog. If you remove the table, it will disappear from the glue. You don't need to do anything else on your end.
The other thing is the fine grained control. So a lot of customers ask hey? Can I have a little bit more fine grained control on? Who got who gets access to what? So let's say you got a good amount of tabular data, and you want to hide some of the columns from some
party whom you're kind of showing the data, then you can do that fine grain control using Aws lake formation. And again, lake formation is also tightly integrated with S 3 table. So that means
if you, during the creation of the S 3 table bucket, if you had that checkmarked of integration, that means it's going to do integration with both the glue as well as the lake formation. And now in the lake formation aws lake formation will give you that fine grained access control on specific users. You have access to which tables they have access to also within tables which columns they have access to.
You can also integrate this with the Identity Center or active directory. So you know, if you have this external identity providers, you can integrate them and then provide the access to those users within those identities as well.
Now, let's look at the
some other ways. You could kind of access the S 3 tables right? So not every customers would use aws services when it comes to analyzing the data. So you could be using some.
you know, on in-house Emr or spark clusters running on Ec 2
or a Flink. A lot of people use their flink streaming services. And if you, if you're using any of those things, we provide you the connector, to connect to the S. 3 table, so you can use the 3rd party tools as well, since it's the S. 3 table is based on the iceberg as long as the it can connect with the iceberg you can. You can work with it.
So the other use case is you know.
especially for indeed, you guys may be doing a lot of this user experience level, like, someone is, you know, putting the resumes. And you're monitoring a lot of this analytics on who opened the resume or who clicked on something. So there's a lot of this click streaming events happening. And you can use the data fire hose to
get those all the streaming event. Store them on your S. 3 tables, so it's stored in the tabular format. And then you can analyze those those data natively from the S 3 table itself, and then, you know, create the dashboards, create any kind of things from from the data once it's stored onto the S 3 table.
So that's that's what I kind of thought of will be a good use case for, indeed.
So that's a lot of information I went toward just to kind of recap. S. 3 table is the fully managed iceberg storage service.
It comes with a lot of benefits of Apache iceberg. Like time travel, transactional semantics. It's got the efficient low level updates and deletions, and also the all the benefits of the s. 3. Right? So that scale factor of S. 3, and the elasticity of S. 3, and the high performance of the s. 3. So it kind of combines the
the, the, both the worlds together into into one thing.
and then all these things you get without the operational overhead, which, if you were doing with the iceberg on the general purpose. S. 3. Bucket. You don't have to do all those operational things of compaction, the snapshot management, the unreferenced file removal tape s. 3 table would take care of that for you.
And then, lastly, we talked about the
tight integration with the glue and the lake formation. So again, from that perspective, you don't have to take care of removing the tables or adding the tables to the glue. It takes care of itself.
When it comes to the fine grain, it's fully integrated with the lake formation and lake formation is integrated with Iam and the other entity providers. So you can kind of give the fine grain control on the tables at the column levels to the S. 3 tables.
And that's pretty much it happy to take any questions. I think we got 2 questions from you. We'll follow up on those 2 questions. One is the slo around compaction, and the other is you know, if there's any way to.
I mean, I get your question about the this 1st one, about the compaction
control. So what what I want to kind of ask our product management team is.
can I schedule the complexion at my predefined interval level? So is there anything coming on roadmap, and it's constantly evolving. We were just on a talk with the with the team. So there's a lot of things happening on the S. 3 table. And you will see a lot more coming in the near future
that I'll hand it over to
Wasif Rauf
Wasif Rauf
31:54
Alrighty!
RK Vyas
RK Vyas
31:55
Or, yeah.
Wasif Rauf
Wasif Rauf
31:57
This one.
RK Vyas
RK Vyas
31:58
Go ahead!
Wasif Rauf
Wasif Rauf
32:00
So, folks, do we have any questions we still have some time left.
Alex Leong
Alex Leong
32:08
I think there are some questions in the in the chat.
Oscar Hernandez
Oscar Hernandez
32:11
Yeah.
RK Vyas
RK Vyas
32:12
Me check the chat.
We used it time so.
Wasif Rauf
Wasif Rauf
32:18
So how so? Rk, there is a question how many copies of history data are kept by, it appears for backup purposes and protection against disk failures.
RK Vyas
RK Vyas
32:30
So inherently. It is similar to the S. 3. So within the region 3 copies.
Wasif Rauf
Wasif Rauf
32:39
Got it.
So 3 copies within a region.
Alright, thank you. Another question is, does S, 3 table only support iceberg with file format.
Okay? Or does it support iceberg with Orc and Ibro as well.
RK Vyas
RK Vyas
32:55
It does support both of them,
orc and arrow as well. However. I'll send you the document on. There are some limitations, and I would say not limitations. But there are some missing features with arrow and Orc type when it comes to like compaction and other things.
so those would come in the future. If you have a need, I would say Orc, and a profile as the requirement.
let us know. Let your Tam know we can find out where the feature is, but there are some features missing right now for those 2 file types, but it is supported.
Wasif Rauf
Wasif Rauf
33:39
Right?
Those were the 2 questions from the chat.
RK Vyas
RK Vyas
33:54
Awesome. I would say, if you haven't tried as 3 tables.
we are planning to release the S. 3 table workshop
by end of this month, hoping crossing fingers. If that gets released. I think, wasif it'll be a good to have that workshop right? So they can kind of do a hands on and feel about the S. 3 tables.
We are going to have the labs for Emr glue, Athena. We're also planning to see if we could add the emr serverless into it. So you would get to utilize. You know all these different technologies with this 3 tables, and see the compaction. See the snapshot retention and everything
with that workshop. It's going to be like 2 h workshop so once it's ready, as if I'll let you know, and then indeed, team can kind of, you know. Take advantage of that workshop.
Wasif Rauf
Wasif Rauf
34:52
For sure. Sounds really good.
Susan
Susan
34:55
Yeah, okay, we can schedule like a couple of times where people can join the workshop and work through it in a 2 h block. That'd be fantastic.
Alex Leong
Alex Leong
35:03
And.
Susan
Susan
35:05
So watch your emails. We'll let you know when that's available.
RK Vyas
RK Vyas
35:08
Really, yeah.
And again, I will say, it's it's the new service.
If you don't have any question, I'll kind of talk about some of the questions which I have got in the past with other people. Does it have like a PI get questions about like versioning? Does it have just like S. 3. General post bucket has versioning, does it have versioning?
We don't have versioning like the S. 3 general purpose bucket. But there's a versioning inside of it which you don't have visibility. You don't control it, you can't enable it, you can't disable it. It's enabled by default and utilize it.
The other questions I get asked is, does it have all the capabilities of
general purpose? S. 3. Bucket
and the answer is, consider the S. 3 tables as a new construct, and don't compare it with the S. 3 general purpose bucket
so you will not have a lot of things which are available in S. 3 general purpose bucket right now, like you can't do a replication. You can't do
peering like. You know this in the S. 3 general purpose bucket. You have different tiers. Right? You can go to a glacier. You can go to a archive tool tier. You can go to deep Archival tier. There's nothing like this. So the only common things between, I would say, there's 3 tables, and there's 3 general purpose. Bucket right now is the resource policy.
And the second thing is the scale and durability of of the S. 3.
Oscar Hernandez
Oscar Hernandez
36:57
And so one question about this. Maybe I think you mentioned it, but maybe I missed it so the sdks have like a different object. Now, it's not like s. 3. Put, but this s. 3 table, something in Python and the other languages. Okay.
RK Vyas
RK Vyas
37:12
Yep.
And also, if you're using aws, cli use the newer aws. Cli. Same thing goes with the SDK. Use the newer SDK, because this is a new feature. So all the new things come in the newer version of the Aws cli and SDK.
Oscar Hernandez
Oscar Hernandez
37:31
Awesome. Thanks.
RK Vyas
RK Vyas
37:33
Sure.
Wasif Rauf
Wasif Rauf
37:36
There's another question in the chat. How does it handle Pii?
Is it automatic?
RK Vyas
RK Vyas
37:44
So when it comes to Pii, consider s. 3 table as
just like you're storing. And from from just a pii perspective, right? Consider the your. You have your own ice, 3 iceberg tables on general purpose. Bucket right? You don't have way to mask the Pii information unless you do something about it.
So same thing applies to the S. 3 tables. Right? We are not going to automatically mask Pii information.
You would have to do something about masking it.
Wasif Rauf
Wasif Rauf
38:25
thank you.
Chris Voss
Chris Voss
38:27
I have a question around Pii. So let's say we have pii that we don't necessarily want to mask, but we are required to delete for legal compliance purposes. When a an owner of that Pii request we delete it. Are there limitations kind of stemming from versioning and time travel to deleting fields? Can we? Can we actually fully delete fields, or will they?
Will they stick around due to time, travel or versioning.
RK Vyas
RK Vyas
38:58
Well, it will stick around because of the time. Travel, however. So so again your all your questions right now, if you're familiar with the iceberg tables on general purpose bucket. If you use that, it will stay the same, because it's the same problem with the with the iceberg. Also. The only way you could kind of do it is I'm kind of thinking it. If it's it's if it's like very sensitive information, you store them into
a separate S 3 table bucket.
and then you keep the retention, the snapshot retention of that to be very low, very small. So that way your sensitive information. When it's deleted. It doesn't give you the time. Travel option to like longer duration.
Does that kind of help.
Chris Voss
Chris Voss
39:54
Yeah, that makes sense. Thank you.
RK Vyas
RK Vyas
39:57
And any of these features. Right? I would. I would always encourage customers.
Don't take this limitations, whatever I'm telling you right now as the limitations. What you have.
if it's something you really need it? And will something really help you? Move away from your self managed iceberg tables? Talk to your temp, and he would know exactly you know how to reach out to the Pm. How to reach out to us.
and also we can have the concept of product feature request. So we can add your custom, influence, and more customers request for those kind of things. Those features will come out faster.
Wasif Rauf
Wasif Rauf
40:42
Absolutely.
Susan
Susan
40:50
I'm just gonna stick how to. If you want to do a product feature request, you can just go to this Wiki page and start the process here.
Well, thank you, everyone for coming to awesome hour. Have a great rest of your day, and again, if you have any questions, reach out, and we can follow up. Thanks, all.