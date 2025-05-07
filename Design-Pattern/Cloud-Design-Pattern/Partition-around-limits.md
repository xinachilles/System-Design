# Partition around limits

Created: 2018-10-07 00:08:05 -0600

Modified: 2018-10-07 00:10:34 -0600

---

Use partitioning to work around database, network, and compute limits In the cloud, all services have limits in their ability to scale up. Azure service limits are documented in Azure subscription and service limits, quotas, and constraints. Limits include number of cores, database size, query throughput, and network throughput. If your system grows sufficiently large, you may hit one or more of these limits. Use partitioning to work around these limits. There are many ways to partition a system, such as: Partition a queue or message bus to avoid limits on the number of requests or the number of concurrent connections. Partition an App Service web app to avoid limits on the number of instances per App Service plan. A database can be partitioned horizontally, vertically, or functionally. In horizontal partitioning, also called sharding, each partition holds data for a subset of the total data set. The partitions share the same data schema. For example, customers whose names start with A--M go into one partition, N--Z into another partition. In vertical partitioning, each partition holds a subset of the fields for the items in the data store. For example, put frequently accessed fields in one partition, and less frequently accessed fields in another. In functional partitioning, data is partitioned according to how it is used by each bounded context in the system. For example, store invoice data in one partition and product inventory data in another. The schemas are independent. to --Design for evolution
