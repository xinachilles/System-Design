# Index

Created: 2019-12-09 11:55:16 -0600

Modified: 2019-12-30 16:52:24 -0600

---

![10:54 Availability patterns 06/22/2017 • 2 minutes to read • Availability defines the proportion of time that the system is functional and working. It will be affected by system errors, infrastructure problems, malicious attacks, and system load. It is usually measured as a percentage of uptime. Cloud applications typically provide users with a service level agreement (SLA), which means that applications must be designed and implemented in a way that maximizes availability. Pattern Health Endpoint Monitoring Queue- Based Load Leveling Summary Implement functional checks in an application that external tools can access through exposed endpoints at regular intervals. Use a queue that acts as a buffer between a task and a service that it invokes in order to smooth intermittent heavy loads. O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image1.png){width="5.635416666666667in" height="12.125in"}



![Throttling 10:55 Control the consumption of resources used by an instance of an application, an individual tenant, or an entire service. Is this page helpful? O Yes 47 No Feedback Send feedback about This product e O This page There is currently no feedback for this document. Page feedback will appear here. English (United States) Previous Version Docs • Blog • Contribute O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image2.png){width="5.635416666666667in" height="12.125in"}



![10:56 bign In Data Management patterns 06/22/2017 • 2 minutes to read Data management is the key element of cloud applications, and influences most of the quality attributes. Data is typically hosted in different locations and across multiple servers for reasons such as performance, scalability or availability, and this can present a range of challenges. For example, data consistency must be maintained, and data will typically need to be synchronized across different locations. Pattern Cache- Aside CQRS Summary Load data on demand into a cache from a data store Segregate operations that read data from operations that update data by using separate interfaces. O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image3.png){width="5.635416666666667in" height="12.125in"}



![Event Sourcing Index Table Materialized View Sharding Static Content Hosting Valet Key 10:56 Use an append-only store to record the full series of events that describe actions taken on data in a domain. Create indexes over the fields in data stores that are frequently referenced by queries. Generate prepopulated views over the data in one or more data stores when the data isn't ideally formatted for required query operations. Divide a data store into a set of horizontal partitions or shards. Deploy static content to a cloud-based storage service that can deliver them directly to the client. Use a token or key that provides clients with restricted direct access to a specific resource or service. IR thic helnf1117 O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image4.png){width="5.635416666666667in" height="12.125in"}



![10:57 docs.microsoft.com Design and Implementation patterns 06/22/2017 • 2 minutes to read • Good design encompasses factors such as consistency and coherence in component design and deployment, maintainability to simplify administration and development, and reusability to allow components and subsystems to be used in other applications and in other scenarios. Decisions made during the design and implementation phase have a huge impact on the quality and the total cost of ownership of cloud hosted applications and services. Pattern Ambassador Summary Create helper services that send network requests on behalf of a consumer service or application. O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image5.png){width="5.635416666666667in" height="12.125in"}



![Anti- Corruption Layer Backends for Frontends CQRS Compute Resource Consolidation External Configuration Store Gateway Aggregation Gateway Offloading 10:57 Implement a facade or adapter layer between a modern application and a legacy system. Create separate backend services to be consumed by specific frontend applications or interfaces. Segregate operations that read data from operations that update data by using separate interfaces. Consolidate multiple tasks or operations into a single computational unit Move configuration information out of the application deployment package to a centralized location. Use a gateway to aggregate multiple individual requests into a single request. Offload shared or specialized service functionality to a gateway O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image6.png){width="5.635416666666667in" height="12.125in"}



![Gateway Routing Leader Election Pipes and Filters Sidecar Static Content Hosting Strangler 10:57 Route requests to multiple services using a single endpoint. Coordinate the actions performed by a collection of collaborating task instances in a distributed application by electing one instance as the leader that assumes responsibility for managing the other instances. Break down a task that performs complex processing into a series of separate elements that can be reused. Deploy components of an application into a separate process or container to provide isolation and encapsulation. Deploy static content to a cloud-based storage service that can deliver them directly to the client. Incrementally migrate a legacy system by gradually O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image7.png){width="5.635416666666667in" height="12.125in"}



![10:58 docs.microsoft.com Management and Monitoring patterns 06/22/2017 • 2 minutes to read • +1 Cloud applications run in a remote datacenter where you do not have full control of the infrastructure or, in some cases, the operating system. This can make management and monitoring more difficult than an on-premises deployment. Applications must expose runtime information that administrators and operators can use to manage and monitor the system, as well as supporting changing business requirements and customization without requiring the application to be stopped or redeployed. Pattern Ambassador Summary Create helper services that send network requests on behalf of a consumer service or application. O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image8.png){width="5.635416666666667in" height="12.125in"}



![Anti- Corruption Layer External Configuration Store Gateway Aggregation Gateway Offloading Gateway Routing Health Endpoint Monitoring Sidecar 10:58 Implement a facade or adapter layer between a modern application and a legacy system. Move configuration information out of the application deployment package to a centralized location. Use a gateway to aggregate multiple individual requests into a single request. Offload shared or specialized service functionality to a gateway proxy. Route requests to multiple services using a single endpoint. Implement functional checks in an application that external tools can access through exposed endpoints at regular intervals. Deploy components of an O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image9.png){width="5.635416666666667in" height="12.125in"}



![Strangler 10:58 Incrementally migrate a legacy system by gradually replacing specific pieces of functionality with new applications and services. Is this page helpful? o Yes 47 No Feedback Send feedback about This product e O This page There is currently no feedback for this document. Page feedback will appear here. English (United States) O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image10.png){width="5.635416666666667in" height="12.125in"}



![10:59 docs.microsoft.com Messaging patterns 10/23/2019 • 2 minutes to read • +2 The distributed nature of cloud applications requires a messaging infrastructure that connects the components and services, ideally in a loosely coupled manner in order to maximize scalability. Asynchronous messaging is widely used, and provides many benefits, but also brings challenges such as the ordering of messages, poison message management, idempotency, and more. Pattern Asynchronous Request- Reply Claim Check Summary Decouple backend processing from a frontend host, where backend processing needs to be asynchronous, but the frontend still needs a clear response. Split a large message into a claim check and a payload to avoid overwhelmina a O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image11.png){width="5.635416666666667in" height="12.125in"}



![Choreography Competing Consumers Pipes and Filters Priority Queue Publisher- Subscriber 10:59 message DUS. Have each component of the system participate in the decision-making process about the workflow of a business transaction, instead of relying on a central point of control. Enable multiple concurrent consumers to process messages received on the same messaging channel. Break down a task that performs complex processing into a series of separate elements that can be reused. Prioritize requests sent to services so that requests with a higher priority are received and processed more quickly than those with a lower priority. Enable an application to announce events to multiple interested consumers asynchronously, without coupling the O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image12.png){width="5.635416666666667in" height="12.125in"}



![Queue-Based Load Leveling Scheduler Agent Supervisor 10:59 Use a queue that acts as a buffer between a task and a service that it invokes in order to smooth intermittent heavy loads. Coordinate a set of actions across a distributed set of services and other remote resources. Is this page helpful? cåYes 47 No Feedback Send feedback about This product e O This page O Loading feedback... O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image13.png){width="5.635416666666667in" height="12.125in"}



![11:03 docs.microsoft.com renorrnance Scalability patterns 08/26/2019 • 2 minutes to read • +1 Performance is an indication of the responsiveness of a system to execute any action within a given time interval, while scalability is ability of a system either to handle increases in load without impact on performance or for the available resources to be readily increased. Cloud applications typically encounter variable workloads and peaks in activity. Predicting these, especially in a multi-tenant scenario, is almost impossible. Instead, applications should be able to scale out within limits to meet peaks in demand, and scale in when demand decreases. Scalability concerns not just compute instances, but other elements such as data storage, messaging infrastructure, and more. Pattern Cache-Aside Summary Load data on demand into a O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image14.png){width="5.635416666666667in" height="12.125in"}



![11:03 docs.microsoft.com CQRS Event Sourcing Index Table Materialized View Priority Queue decision-making process about the workflow of a business transaction, instead of relying on a central point of control. Segregate operations that read data from operations that update data by using separate interfaces. Use an append-only store to record the full series of events that describe actions taken on data in a domain. Create indexes over the fields in data stores that are frequently referenced by queries. Generate prepopulated views over the data in one or more data stores when the data isn't ideally formatted for required query operations. Prioritize requests sent to services so that requests O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image2.png){width="5.635416666666667in" height="12.125in"}



![Queue-Based Load Leveling Sharding Static Content Hosting Throttling 11:03 Use a queue that acts as a buffer between a task and a service that it invokes in order to smooth intermittent heavy loads. Divide a data store into a set of horizontal partitions or shards. Deploy static content to a cloud-based storage service that can deliver them directly to the client. Control the consumption of resources used by an instance of an application, an individual tenant, or an entire service. Is this page helpful? o Yes 47 No Feedback Send feedback about O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image15.png){width="5.635416666666667in" height="12.125in"}



![11:04 docs.microsoft.com Resiliency patterns 06/22/2017 • 2 minutes to read • Resiliency is the ability of a system to gracefully handle and recover from failures. The nature of cloud hosting, where applications are often multi-tenant, use shared platform services, compete for resources and bandwidth, communicate over the Internet, and run on commodity hardware means there is an increased likelihood that both transient and more permanent faults will arise. Detecting failures, and recovering quickly and efficiently, is necessary to maintain resiliency. Pattern Bulkhead Circuit Breaker Summary Isolate elements of an application into pools so that if one fails, the others will continue to function. Handle faults that might take a variable amount of O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image16.png){width="5.635416666666667in" height="12.125in"}



![Compensating Transaction Health Endpoint Monitoring Leader Election Queue-Based Load Leveling Retry 11:04 Undo the work performed by a series of steps, which together define an eventually consistent operation. Implement functional checks in an application that external tools can access through exposed endpoints at regular intervals. Coordinate the actions performed by a collection of collaborating task instances in a distributed application by electing one instance as the leader that assumes responsibility for managing the other instances. Use a queue that acts as a buffer between a task and a service that it invokes in order to smooth intermittent heavy loads. Enable an application to handle anticipated, O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image17.png){width="5.635416666666667in" height="12.125in"}



![Scheduler Agent Supervisor 11:04 failed. Coordinate a set of actions across a distributed set of services and other remote resources. Is this page helpful? ± Yes 47 No Feedback Send feedback about This product e O This page There is currently no feedback for this document. Page feedback will appear here. English (United States) 1 c  / arc lr-•tn n Arc • O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image18.png){width="5.635416666666667in" height="12.125in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image19.png){width="5.635416666666667in" height="12.125in"}



![Valet Key 11:07 application or service, validates and sanitizes requests, and passes requests and data between them. Use a token or key that provides clients with restricted direct access to a specific resource or service. Is this page helpful? O Yes 47 No Feedback Send feedback about This product e O This page There is currently no feedback for this document. Page feedback will appear here. O ](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image20.png){width="5.635416666666667in" height="12.125in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image21.png){width="5.03125in" height="1.0520833333333333in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image22.png){width="4.583333333333333in" height="0.59375in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image23.png){width="4.4375in" height="0.71875in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image24.png){width="4.364583333333333in" height="0.8854166666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image25.png){width="5.03125in" height="0.78125in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image26.png){width="4.697916666666667in" height="0.8541666666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image27.png){width="4.104166666666667in" height="0.6875in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image28.png){width="4.520833333333333in" height="0.59375in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image29.png){width="4.375in" height="0.8645833333333334in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image30.png){width="4.697916666666667in" height="0.8645833333333334in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image31.png){width="4.125in" height="0.6979166666666666in"}![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image32.png){width="4.125in" height="0.9479166666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image33.png){width="4.1875in" height="0.9479166666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image34.png){width="4.229166666666667in" height="0.6145833333333334in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image35.png){width="3.9166666666666665in" height="1.0625in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image36.png){width="4.71875in" height="0.8854166666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image37.png){width="4.3125in" height="0.7083333333333334in"}![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image38.png){width="3.9375in" height="1.03125in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image39.png){width="3.5104166666666665in" height="0.6458333333333334in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image40.png){width="4.083333333333333in" height="0.625in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image41.png){width="4.03125in" height="0.6354166666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image42.png){width="3.4895833333333335in" height="0.90625in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image43.png){width="4.354166666666667in" height="0.9479166666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image44.png){width="4.197916666666667in" height="0.8229166666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image45.png){width="4.395833333333333in" height="0.7395833333333334in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image46.png){width="4.802083333333333in" height="0.8541666666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image47.png){width="4.604166666666667in" height="0.65625in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image48.png){width="4.385416666666667in" height="0.5833333333333334in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image49.png){width="3.75in" height="0.6979166666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image50.png){width="4.5in" height="0.8645833333333334in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image51.png){width="5.322916666666667in" height="1.1041666666666667in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image52.png){width="4.25in" height="0.7916666666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image53.png){width="0.23958333333333334in" height="0.5in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image54.png){width="4.208333333333333in" height="1.03125in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image55.png){width="3.59375in" height="0.6979166666666666in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image56.png){width="4.385416666666667in" height="0.75in"}



![](../../media/Availability-or-Reliabiity-Availability-or-Reliabiity-Index-image57.png){width="4.1875in" height="0.8229166666666666in"}

























































