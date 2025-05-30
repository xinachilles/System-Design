# Spark 



---

And Hadoop doesn't provide any optimization for caching the data, store all result in HDFS. start from HDFS and end from HDFS, Doesn't support any sort of data caching in between iterations.



for example, for the word count or page rank .There was two different MapReduce jobs. There was the propagation phase, and then there was the aggregation phase.



And you would run the first job, second job, again first job, second job, first job, second job, you would keep doing that.





So now, Hadoop offered a lot of large scale capabilities. it can process the data. a chuck of data every five minutes, every hour. But there was a growing need to provide data in real time for analytics use cases





Now the solution that Spark provides is a data and computation abstraction called Resilient Distributed Datasets (RDD).Resilient, this means that once you load it into memory, it remains into memory, until you say, okay I'm going to get rid of this.



It's also distributed. So when you have a cluster, you, as the programmer, can think of a data set, an RDD, as one logical thing. In reality pieces of this RDD are spread all around the cluster,



okay.



So in your algorithm you say RDD1 load from disk.



RDD2 is RDD1 filtered by this parameter.



RDD3 is RDD2 sorted.



RDD4 is RDD3 something else.



All of these RDDs are distributed across the cluster and



the framework manages them.





RDD do the lazy evaluation







How RDD





The dates are the RDD starts from the HDFS, the file that data stored on HDFS File.



And then once we load that into memory, we do a filter, and it becomes a Filtered RDD. And then we do a map on it and it becomes a Mapped RDD.





So the framework kind of just [writes down these transformation information along with each RDD.]{.underline}



If a certain part of the cluster fails, that means that an RDD is affected,



in the worst case, what we can do is we can load up data from HDFS file again, [apply these filters and these maps, and all the transformations necessary,]{.underline} and we can recompute an RDD anytime that you require.



And that's how Spark provides fault tolerance. It keeps lineage of transformation, and if there's a failure it can redo the computation.





the different between storm and spark is the data is not covering to RDD and fail those data will be lost














