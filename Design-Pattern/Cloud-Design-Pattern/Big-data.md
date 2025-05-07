# Big data 

Created: 2018-09-25 22:35:41 -0600

Modified: 2018-09-26 14:37:06 -0600

---

Big data

is for following types of workload



Most big data architectures include some or all of the following components:

1.  data source:



- Application data stores, such as relational databases.
- Static files produced by applications, such as web server log files.
- Real-time data sources, such as IoT devices.



2.  data storage:



- Data storage: called a*data lake*.

3.  Batch processing:



- Batch processing: Because the data sets are so large, often a big data solution must process data files using long-running batch jobs to filter, aggregate, and otherwise prepare the data for analysis. Usually these jobs involve reading source files, processing them, and writing the output to new files. using Hive, Pig, or custom Map/Reduce jobs in an HDInsight Hadoop cluster, or using Java, Scala, or Python programs in an HDInsight Spark cluster.



- Real-time message ingestion: If the solution includes real-time sources, the architecture must include a way to capture and store real-time messages for stream processing. This might be a simple data store, where incoming messages are dropped into a folder for processing. However, many solutions need a message ingestion store to act as a buffer for messages, and to support scale-out processing, reliable delivery, and other message queuing semantics. Options include Azure Event Hubs, Azure IoT Hubs, and Kafka.



- Stream processing: After capturing real-time messages, the solution must process them by filtering, aggregating, and otherwise preparing the data for analysis. The processed stream data is then written to an output sink. You can also use open source Apache streaming technologies like Storm and Spark Streaming in an HDInsight cluster.



- Analytical data store: Many big data solutions prepare data for analysis and then serve the processed data in a structured format that can be queried using analytical tools. The analytical data store used to serve these queries can be a Kimball-style relational data warehouse, as seen in most traditional business intelligence (BI) solutions. Alternatively, the data could be presented through a low-latency NoSQL technology such as HBase, or an interactive Hive database that provides a metadata abstraction over data files in the distributed data store.



- Analysis and reporting: The goal of most big data solutions is to provide insights into the data through analysis and reporting.



- Orchestration: Most big data solutions consist of repeated data processing operations, encapsulated in workflows, that transform source data, move data between multiple sources and sinks, load the processed data into an analytical data store, or push the results straight to a report or dashboard. To automate these workflows, you can use an orchestration technology such Azure Data Factory or Apache Oozie and Sqoop.



- .

IoT architecture

Internet of Things (IoT) is a specialized subset of big data solutions. The following diagram shows a possible logical architecture for IoT. The diagram emphasizes the event-streaming components of the architecture.

![Devices Devices Field Gateway Command and control Cloud Gatew ay Provisioning API Oev•ce Reg&ry Cold firage Batch analvtics gukend Stream processing Hot path analytics Notifications ](../../media/Design-Pattern-Cloud-Design-Pattern-Big-data-image1.png){width="10.083333333333334in" height="4.354166666666667in"}

Thecloud gatewayingests device events at the cloud boundary, using a reliable, low latency messaging system.



Devices might send events directly to the cloud gateway, or through afield gateway. A field gateway is a specialized device or software, usually colocated with the devices, that receives events and forwards them to the cloud gateway.



The field gateway might also preprocess the raw device events, performing functions such as filtering, aggregation, or protocol transformation.

After ingestion, events go through one or morestream processorsthat can route the data (for example, to storage) or perform analytics and other processing.

The following are some common types of processing. (This list is certainly not exhaustive.)

- Writing event data to cold storage, for archiving or batch analytics.
- Hot path analytics, analyzing the event stream in (near) real time, to detect anomalies, recognize patterns over rolling time windows, or trigger alerts when a specific condition occurs in the stream.
- Handling special types of non-telemetry messages from devices, such as notifications and alarms.
- Machine learning.

The boxes that are shaded gray show components of an IoT system that are not directly related to event streaming, but are included here for completeness.

- Thedevice registryis a database of the provisioned devices, including the device IDs and usually device metadata, such as location.
- Theprovisioning APIis a common external interface for provisioning and registering new devices.
- Some IoT solutions allowcommand and control messagesto be sent to devices.

This section has presented a very high-level view of IoT, and there are many subtleties and challenges to consider. For a more detailed reference architecture and discussion, see the[Microsoft Azure IoT Reference Architecture](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)(PDF download).

