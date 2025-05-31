# Batch process and  no relational data stores
---
The source data is loaded into data storage, either by the source application itself or by another job or workflow.

## Challenges

### Data format and encoding

- For example, source files might use a mix of UTF-16 and UTF-8 encoding, or contain unexpected delimiters (space versus tab), or include unexpected characters.

- Another common example is text fields that contain tabs, spaces, or commas that are interpreted as delimiters.

- Data loading and parsing logic must be flexible enough to detect and handle these issues.

- Orchestrating time slices. Often source data is placed in a folder hierarchy that reflects processing windows, organized by year, month, day, hour, and so on. In some cases, data may arrive late. For example, suppose that a web server fails, and the logs for March 7th don't end up in the folder for processing until March 9th. Are they just ignored because they're too late? Can the downstream processing logic handle out-of-order records?

## Architecture

- Data storage: Azure Storage Blob Containers;Azure Data Lake Store.

- Batch processing. Usually these jobs involve reading source files, processing them (filter, aggregate, and otherwise prepare the data for analysis), and writing the output to new files.

- Analytical data store. Many big data solutions are designed to prepare data for analysis and then serve the processed data in a structured format that can be queried using analytical tools.

- Orchestration. With batch processing, typically some orchestration is required to migrate or copy the data into your data storage, batch processing, analytical data store, and reporting layers.


## Technology choices

- The following technologies are recommended choices for batch processing solutions in Azure.

- Analytical data store: Spark SQL,HBase,Hive

- Analytics and reporting

## Orchestration

 - Azure Data Factory. These activities can initiate data copy operations as well as Hive, Pig, MapReduce, or Spark jobs in on-demand HDInsight clusters;

- Oozie and Sqoop. Oozie is a job automation engine for the Apache Hadoop ecosystem and can be used to initiate data copy operations as well as Hive, Pig, and MapReduce jobs to process data and Sqoop jobs to copy data between HDFS and SQL databases.

- For more information, see Pipeline orchestration

## Real time processing

- Real time processing deals with streams of data that are captured in real-time and processed with minimal latency to generate real-time (or near-real-time) reports or automated responses.

- For example, a real-time traffic monitoring solution might use sensor data to detect high traffic volumes. This data could be used to dynamically update a map to show congestion,
or automatically initiate high-occupancy lanes or other traffic management systems.

- Processed data is often written to an analytical data store, which is optimized for analytics and visualization.

- The processed data can also be ingested directly into the analytics and reporting layer for analysis, business intelligence, and real-time dashboard visualization.


## Architecture
A real-time processing architecture has the following logical components.

- Real-time message ingestion. The architecture must include a way to capture and store real-time messages to be consumed by a stream processing consumer.

- Stream processing. After capturing real-time messages, the solution must process them by filtering, aggregating, and otherwise preparing the data for analysis.

- Analytical data store. Many big data solutions are designed to prepare data for analysis and then serve the processed data in a structured format that can be queried using analytical tools.

- Real-time message ingestion:Apache Kafka.

### Data storage

- Azure Storage Blob Containers or Azure Data Lake Store.
- Stream processing Apache Storm,Apache Spark

#### Analytical data store
- SQL Data Warehouse, HBase, Spark, or Hive.

#### Analytics and reporting

In a purely real-time solution, most of the processing orchestration is managed by the message ingestion and stream processing components.

However, in a lambda architecture that combines batch processing and real-time processing,

you may need to use an orchestration framework such as Azure Data Factory or Apache Oozie and Sqoop to manage batch workflows for captured real-time data.

### Non-relational data and NoSQL

#### Document data stores

A document data store manages a set of named string fields and object data values in an entity referred to as a document.

These data stores typically store data in the form of JSON documents.

The application can retrieve documents by using the document key.

Some document databases create the document key automatically.

The application can also query documents based on the value of one or more fields.

Some document databases support indexing to facilitate fast lookup of documents based on one or more indexed fields.

Many document databases support in-place updates, enabling an application to modify the values of specific fields in a document without rewriting the entire document.

Read and write operations over multiple fields in a single document are usually atomic.

#### Columnar data stores

A columnar or column-family data store organizes data into columns and rows. In its simplest form, a column-family data store can appear very similar to a relational database,

The real power of a column-family database lies in its denormalized approach

columns are divided into groups known as column families.

Each column family holds a set of columns that are logically related and are typically retrieved or manipulated as a unit.

The following diagram shows an example with two column families, Identity and Contact Info. The data for a single entity has the same row key in each column family.

most column-family databases physically store data in key order, rather than by computing a hash.

The row key is considered the primary index

Some implementations allow you to create secondary indexes over specific columns in a column family.

Secondary indexes let you retrieve data by columns value, rather than row key.

On disk, all of the columns within a column family are stored together in the same file, with a certain number of rows in each file.

With large data sets, this approach creates a performance benefit by reducing the amount of data that needs to be read from disk when only a few columns are queried together at a time.


Read and write operations for a row are usually atomic within a single column family

#### HBase in HDInsight


Key/value data stores

A key/value store is essentially a large hash table. You associate each data value with a unique key, and the key/value store uses this key to store the data by using an appropriate hashing function.

The hashing function is selected to provide an even distribution of hashed keys across the data storage.

key / value structure is very simple


Key/value stores are highly optimized for applications performing simple lookups using the value of the key, or by a range of keys,

but are less suitable for systems that need to query data across different tables of keys/values, such as joining data across multiple tables.

A single key/value store can be extremely scalable, as the data store can easily distribute data across multiple nodes on separate machines.



Key/value stores are also not optimized for scenarios where querying or filtering by non-key values is important, rather than performing lookups based only on keys.

For example, with a relational database, you can find a record by using a WHERE clause to filter the non-key columns,

but key/values stores usually do not have this type of lookup capability for values, or if they do it requires a slow scan of all values.







Graph data stores

A graph data store manages two types of information, nodes and edges. Nodes represent entities, and edges specify the relationships between these entities.

Both nodes and edges can have properties that provide information about that node or edge, similar to columns in a table. Edges can also have a direction indicating the nature of the relationship.



The purpose of a graph data store is to allow an application to efficiently perform queries that traverse the network of nodes and edges, and to analyze the relationships between entities.

The following diagram shows an organization's personnel data structured as a graph. The entities are employees and departments, and the edges indicate reporting relationships and the department in which employees work. In this graph, the arrows on the edges show the direction of the relationships.





This structure makes it straightforward to perform queries such as "Find all employees who report directly or indirectly to Sarah" or "Who works in the same department as John?" For large graphs with lots of entities and relationships, you can perform very complex analyses very quickly. Many graph databases provide a query language that you can use to traverse a network of relationships efficiently.







Time series data stores

Time series data is a set of values organized by time, and a time series data store is optimized for this type of data.

Time series data stores must support a very high number of writes, as they typically collect large amounts of data in real time from a large number of sources.

Time series data stores are optimized for storing telemetry data. Scenarios include IoT sensors or application/system counters. Updates are rare, and deletes are often done as bulk operations.



Although the records written to a time series database are generally small, there are often a large number of records, and total data size can grow rapidly.

Time series data stores also handle out-of-order and late-arriving data, automatic indexing of data points, and optimizations for queries described in terms of windows of time.

This last feature enables queries to run across millions of data points and multiple data streams quickly



For more information, see Time series solutions







Object data stores

Object data stores are optimized for storing and retrieving large binary objects or blobs such as images, text files, video and audio streams, large application data objects and documents, and virtual machine disk images. An object consists of the stored data, some metadata, and a unique ID for accessing the object. Object stores are designed to support files that are individually very large, as well provide large amounts of total storage to manage all files.





Some object data stores replicate a given blob across multiple server nodes, which enables fast parallel reads.

This in turn enables the scale-out querying of data contained in large files, because multiple processes, typically running on different servers, can each query the large data file simultaneously.



One special case of object data stores is the network file share. Using file shares enables files to be accessed across a network using standard networking protocols like server message block (SMB). Given appropriate security and concurrent access control mechanisms, sharing data in this way can enable distributed services to provide highly scalable data access for basic, low level operations such as simple read and write requests.





For example, you might have text files stored in a file system. Finding a file by its file path is quick,

but searching based on the contents of the file would require a scan of all of the files, which is slow.

An external index lets you create secondary search indexes and then quickly find the path to the files that match your criteria.

External index data stores

External index data stores provide the ability to search for information held in other data stores and services.

An external index acts as a secondary index for any data store, and can be used to index massive volumes of data and provide near real-time access to these indexes.





The indexes are created by running an indexing process. This can be performed using a pull model, triggered by the data store, or using a push model, initiated by application code.

Indexes can be multidimensional and may support free-text searches across large volumes of text data.



External index data stores are often used to support full text and web based search.
