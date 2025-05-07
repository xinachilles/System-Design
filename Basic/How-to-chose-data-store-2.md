# How to chose data store 2

Created: 2018-10-01 23:29:13 -0600

Modified: 2020-01-06 18:23:15 -0600

---

**Functional requirements**

**Data format.**



What type of data are you intending to store? Common types include transactional data, JSON objects, telemetry, search indexes, or flat files.



Data size.

How large are the entities you need to store? Will these entities need to be maintained as a single document, or can they be split across multiple documents, tables, collections, and so forth?



Scale and structure.

What is the overall amount of storage capacity you need? Do you anticipate partitioning your data?



~~Data relationships.~~

~~Will your data need to support one-to-many or many-to-many relationships? Are relationships themselves an important part of the data?~~





Consistency model. How important is it for updates made in one node to appear in other nodes ? Do you need ACID guarantees for transactions?



~~Concurrency. What kind of concurrency mechanism do you want to use when updating and synchronizing data? Will the application perform many updates that could potentially conflict. If so, you may require record locking and pessimistic concurrency control.~~



~~Alternatively, can you support optimistic concurrency controls? If so, is simple timestamp-based concurrency control enough, or do you need the added functionality of multi-version concurrency control?~~



Data lifecycle. Is the data write-once, read-many? Can it be moved into cool or cold storage?



~~Other supported features. Do you need any other specific features, such as schema validation, aggregation, indexing, full-text search, MapReduce, or other query capabilities?~~



Non-functional requirements

Performance and scalability.



~~What are your data performance requirements? Do you have specific requirements for data ingestion rates and data processing rates? What are the acceptable response times for querying and aggregation of data once ingested?~~



How large will you need the data store to scale up? Is your workload more read-heavy or write-heavy?



~~Reliability. What overall SLA do you need to support? What level of fault-tolerance do you need to provide for data consumers? What kind of backup and restore capabilities do you need?~~



Replication. Will your data need to be distributed among multiple replicas or regions? What kind of data replication capabilities do you require?



~~Portability. Will your data need to migrated to on-premises, external datacenters, or other cloud hosting environments?~~



~~Licensing. Do you have a preference of a proprietary versus OSS license type? Are there any other external restrictions on what type of license you can use?~~

~~Overall cost. What is the overall cost of using the service within your solution? How many instances will need to run, to support your uptime and throughput requirements? Consider operations costs in this calculation. One reason to prefer managed services is the reduced operational cost.~~

~~Cost effectiveness. Can you partition your data, to store it more cost effectively? For example, can you move large objects out of an expensive relational database into an object store?~~



Relational database management systems (RDBMS)



Multiple operations have to be completed in a single transaction

Strong integration with reporting tools is required.

Relationships are enforced using database constraints.

Indexes are used to optimize query performance.



Data type : Data is highly normalized.

Database schemas are required and enforced.

Many-to-many relationships between data entities in the database.



Data requires strong consistency. Transactions operate in a way that ensures all data are 100% consistent for all users and processes.



Examples

Line of business (human capital management, customer relationship management, enterprise resource planning)

Inventory management

Reporting database

Accounting

Asset management

Fund management

Order management



Document databases

No object-relational impedance mismatch. Documents can better match the object structures used in application code.

Optimistic concurrency is more commonly used.

Data must be modified and processed by consuming application.

Data requires index on multiple fields.

Individual documents are retrieved and written as a single block.

Data type

Data can be managed in de-normalized way.

Size of individual document data is relatively small.

Each document type can use its own schema.

Documents can include optional fields.

Document data is semi-structured, meaning that data types of each field are not strictly defined.

Data aggregation is supported.

Examples

Product catalog

User accounts

Bill of materials

Personalization

Content management

Operations data

Inventory management

Transaction history data

Materialized view of other NoSQL stores. Replaces file/BLOB indexing.





Key/value stores

Workload

Data is identified and accessed using a single ID key, like a dictionary.

Massively scalable.

No joins, lock, or unions are required.

No aggregation mechanisms are used.

Secondary indexes are generally not used.

Data type

Data size tends to be large.

Each key is associated with a single value, which is an unmanaged data BLOB.

There is no schema enforcement.

No relationships between entities.

Examples

Data caching

Session management

User preference and profile management

Product recommendation and ad serving

Dictionaries





Graph databases

Workload

The relationships between data items are very complex, involving many hops between related data items.

The relationship between data items are dynamic and change over time.

Relationships between objects are first-class citizens, without requiring foreign-keys and joins to traverse.

Data type

Data is comprised of nodes and relationships.

Nodes are similar to table rows or JSON documents.

Relationships are just as important as nodes, and are exposed directly in the query language.

Composite objects, such as a person with multiple phone numbers, tend to be broken into separate, smaller nodes, combined with traversable relationships

Examples

Organization charts

Social graphs

Fraud detection

Analytics

Recommendation engines





Column-family databases

Most column-family databases perform write operations extremely quickly.

Update and delete operations are rare.

Designed to provide high throughput and low-latency access.

Supports easy query access to a particular set of fields within a much larger record.

Massively scalable.

Data type

Data is stored in tables consisting of a key column and one or more column families.

Specific columns can vary by individual rows.

Individual cells are accessed via get and put commands

Multiple rows are returned using a scan command.

Examples

Recommendations

Personalization

Sensor data

Telemetry

Messaging

Social media analytics

Web analytics

Activity monitoring

Weather and other time-series data
