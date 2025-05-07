# traditional database 

Created: 2018-10-13 22:31:21 -0600

Modified: 2018-10-13 22:31:48 -0600

---

OLTP



Transactional data is information that tracks the interactions related to an organization's activities.

These interactions are typically business transactions, such as payments received from customers, payments made to suppliers, products moving through inventory, orders taken, or services delivered.







OLTP systems record business interactions as they occur in the day-to-day operation of the organization, and support querying of this data to make inferences.





Transactions typically need to be atomic and consistent.



Atomicity means that an entire transaction always succeeds or fails as one unit of work, and is never left in a half-completed state.

If a transaction cannot be completed, the database system must roll back any steps that were already done as part of that transaction.



In a traditional RDBMS, this rollback happens automatically if a transaction cannot be completed.





Transactional databases can support strong consistency for transactions using various locking strategies, such as pessimistic locking,

to ensure that all data is strongly consistent within the context of the enterprise, for all users and processes.



ETL

Typical use cases for ELT fall within the big data realm. For example, you might start by extracting all of the source data to flat files in scalable storage such as Hadoop distributed file system (HDFS) or Azure Data Lake Store. Technologies such as Spark, Hive, or PolyBase can then be used to query the source data. The key point with ELT is that the data store used to perform the transformation is the same data store where the data is ultimately consumed. This data store reads directly from the scalable storage, instead of loading the data into its own proprietary storage. This approach skips the data copy step present in ETL, which can be a time consuming operation for large data sets.



In practice, the target data store is a data warehouse using either a Hadoop cluster (using Hive or Spark) or a SQL Data Warehouse.



In general, a schema is overlaid on the flat file data at query time and stored as a table, enabling the data to be queried like any other table in the data store.

These are referred to as external tables because the data does not reside in storage managed by the data store itself, but on some external scalable storage.









Relevant Azure service:





For example, you might start by extracting all of the source data to flat files in scalable storage such as Hadoop distributed file system (HDFS) or Azure Data Lake Store.

Technologies such as Spark, Hive, or PolyBase can then be used to query the source data.

The key point with ELT is that the data store used to perform the transformation is the same data store where the data is ultimately consumed.

This data store reads directly from the scalable storage, instead of loading the data into its own proprietary storage. This approach skips the data copy step present in ETL, which can be a time consuming operation for large data sets.



In practice, the target data store is a data warehouse using either a Hadoop cluster (using Hive or Spark) or a SQL Data Warehouse.

In general, a schema is overlaid on the flat file data at query time and stored as a table, enabling the data to be queried like any other table in the data store. These are referred to as external tables because the data does not reside in storage managed by the data store itself, but on some external scalable storage.



The data store only manages the schema of the data and applies the schema on read.

For example, a Hadoop cluster using Hive would describe a Hive table where the data source is effectively a path to a set of files in HDFS.

In SQL Data Warehouse, PolyBase can achieve the same result --- creating a table against data stored externally to the database itself.

Once the source data is loaded, the data present in the external tables can be processed using the capabilities of the data store.

In big data scenarios, this means the data store must be capable of massively parallel processing (MPP),

which breaks the data into smaller chunks and distributes processing of the chunks across multiple machines in parallel.



The final phase of the ELT pipeline is typically to transform the source data into a final format



date warehouse



A data warehouse is a central, organizational, relational repository of integrated data from one or more disparate sources, across many or all subject areas. Data warehouses store current and historical data and are used for reporting and analysis of the data in different ways.



Data warehousing in Azure



To move data into a data warehouse, it is extracted on a periodic basis from various sources that contain important business information.

As the data is moved, it can be formatted, cleaned, validated, summarized, and reorganized. Alternately, the data can be stored in the lowest level of detail, with aggregated views provided in the warehouse for reporting. In either case, the data warehouse becomes a permanent storage space for data used for reporting, analysis, and forming important business decisions using business intelligence (BI) tools.





When to use this solution

Choose a data warehouse when you need to turn massive amounts of data from operational systems into a format that is easy to understand, current, and accurate.



Data warehouses are optimized for read access, resulting in faster report generation compared to running reports against the source transaction system.



In addition, data warehouses provide the following benefits:



All historical data from multiple sources can be stored and accessed from a data warehouse as the single source of truth.



You can improve data quality by cleaning up data as it is imported into the data warehouse, providing more accurate data as well as providing consistent codes and descriptions.

Reporting tools do not compete with the transactional source systems for query processing cycles. A data warehouse allows the transactional system to focus predominantly on handling writes, while the data warehouse satisfies the majority of read requests.

A data warehouse can help consolidate data from different software.

Data mining tools can help you find hidden patterns using automatic methodologies against data stored in your warehouse.

Data warehouses make it easier to provide secure access to authorized users, while restricting access to others. There is no need to grant business users access to the source data, thereby removing a potential attack vector against one or more production transaction systems.

Data warehouses make it easier to create business intelligence solutions on top of the data, such as OLAP cubes.













For a large data set, is the data source structured or unstructured? Unstructured data may need to be processed in a big data environment such as Spark on HDInsight, Azure Databricks, Hive LLAP on HDInsight, or Azure Data Lake Analytics. All of these can serve as ELT (Extract, Load, Transform) and ETL (Extract, Transform, Load) engines. They can output the processed data into structured data, making it easier to load into SQL Data Warehouse or one of the other options. For structured data, SQL Data Warehouse has a performance tier called Optimized for Compute, for compute-intensive workloads requiring ultra-high performance.



Do you want to separate your historical data from your current, operational data? If so, select one of the options where orchestration is required.

These are standalone warehouses optimized for heavy read access, and are best suited as a separate historical data store.



Do you need to integrate data from several sources, beyond your OLTP data store? If so, consider options that easily integrate multiple data sources.



Do you prefer a relational data store? If so, narrow your options to those with a relational data store,

but also note that you can use a tool like PolyBase to query non-relational data stores if needed. If you decide to use PolyBase, however, run performance tests against your unstructured data sets for your workload.

Do you need to support a large number of concurrent users and connections? The ability to support a number of concurrent users/connections depends on several factors.



SQL Server allows a maximum of 32,767 user connections. When running on a VM, performance will depend on the VM size and other factors.

SQL Data Warehouse has limits on concurrent queries and concurrent connections. For more information, see Concurrency and workload management in SQL Data Warehouse. Consider using complementary services, such as Azure Analysis Services, to overcome limits in SQL Data Warehouse.

What sort of workload do you have? In general, MPP-based warehouse solutions are best suited for analytical, batch-oriented workloads. If your workloads are transactional by nature, with many small read/write operations or multiple row-by-row operations, consider using one of the SMP options. One exception to this guideline is when using stream processing on an HDInsight cluster, such as Spark Streaming, and storing the data within a Hive table.


