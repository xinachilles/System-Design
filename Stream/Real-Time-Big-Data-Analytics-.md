# Real Time Big Data Analytics with Kafka, Storm & HBase

Created: 2018-01-31 11:48:08 -0600

Modified: 2018-02-02 11:32:26 -0600

---

there are various points of spouts that we would use in terms of advertising data, audience data, other analytics data that we have internally.





And then we have bolts which can process different types of data, and we can do transformations, filters, aggregations, or joins.





So it actually gets some data that it come from Hadoop, and then also a mix of data coming from Storm.



So that was the benefit, is you actually could get some data in real-time and also wanted to take advantage of these Hadoop systems that we had built which had a little bit more guarantee and a higher data quality.



So if someone is running a query at, let's say, may five of an hour. They might only get data that is coming for that hour from the speed layer. if they processed at 30 minutes after the hour. By then there might be some data coming from Hadoop. So it actually gets some data that it come from Hadoop, and then also a mix of data coming from Storm.

So the technology that we've been using the last couple of years is the technology called Druid.



In Druid supports batch ingestion as well as real-time ingestion.



So you can see here there's no, at the front-end of the data system, there's no Hadoop. There's this Storm that processes data that would go then to down-streamers, such as Druid and HDFS.



Now one thing is we still would have Hadoop in play, because there's some things, especially right now, Storm isn't as cost effective for doing large scale

daily, monthly aggregations of data. And we would still use Hadoop for that, but we still would get the benefit of having

Storm be the front-end of our data system. Because it would be able to give us data and the order of seconds.


