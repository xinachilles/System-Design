# design principles :

Created: 2018-10-09 23:31:11 -0600

Modified: 2018-10-10 08:23:29 -0600

---

Relational databases are very good at what they do --- providing ACID guarantees for transactions over relational data. But they come with some costs:



Queries may require expensive joins.

Data must be normalized and conform to a predefined schema (schema on write).

Lock contention may impact performance





Use asynchronous messaging. Asynchronous messaging is a way to decouple the message producer from the consumer.

The producer does not depend on the consumer responding to the message or taking any particular action.

With a pub/sub architecture, the producer may not even know who is consuming the message. New services can easily consume the messages without any modifications to the producer.



Don't build domain knowledge into a gateway. Gateways can be useful in a microservices architecture, for things like request routing, protocol translation, load balancing, or authentication.

However, the gateway should be restricted to this sort of infrastructure functionality.

It should not implement any domain knowledge, to avoid becoming a heavy dependency.





Example showing data using a relational database



A data store that organizes data using the relational model is referred to as a relational database.



Primary keys uniquely identify rows within a table.

Foreign key fields are used in one table to refer to a row in another table by referencing the primary key of the other table.

Foreign keys are used to ensure that the referenced rows are not altered or deleted while the referencing row depends on them.





Unique constraints ensure that all values in a column are unique.







To improve query performance, relational databases use indexes. Primary indexes, which are used by the primary key, define the order of the data as it sits on disk.

Secondary indexes provide an alternative combination of fields, so the desired rows can be queried efficiently, without having to re-sort the entire data on disk.



Because relational databases enforce referential integrity, scaling a relational database can become challenging.

That's because any query or insert operation might touch any number of tables.

You can scale out a relational database by sharding the data, but this requires careful design of the schema. For more information, see Sharding pattern.
