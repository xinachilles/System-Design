# Sql vs no sql



---

<https://www.educative.io/courses/grokking-the-system-design-interview/YQlK1mDPgpK>



**Storage:** SQL stores data in tables where each row represents an entity and each column represents ~~a data point about that entity;~~ property of entry



for example, if we are storing a car entity in a table, different columns could be 'Color', 'Make', 'Model', and so on.



NoSQL databases have different data storage models. The main ones are key-value, document, graph, and columnar.



**Schema:** In SQL, each record conforms to a fixed schema, meaning the columns must be decided and chosen before data entry and ~~each row must have data for each column.~~ The schema can be altered later, but it involves modifying the whole database and going offline.



In NoSQL, schemas are dynamic. Columns can be added on the fly and each 'row' (or equivalent) doesn't have to contain data for each 'column.





**Scalability:** In most common situations, SQL databases are vertically scalable, i.e., by increasing the horsepower (higher Memory, CPU, etc.) of the hardware, which can get very expensive.



It is possible to scale a relational database across multiple servers, but this is a challenging and time-consuming process

(masters and slave) .



On the other hand, NoSQL databases are horizontally scalable, meaning we can add more servers easily in our NoSQL database infrastructure to handle a lot of traffic. Any cheap commodity hardware or cloud instances can host NoSQL databases, thus making it a lot more cost-effective than vertical scaling. A lot of NoSQL technologies also distribute data across servers automatically.





**Reliability or ACID Compliancy (Atomicity, Consistency, Isolation, Durability):** The vast majority of relational databases are ACID compliant. So, when it comes to data reliability and safe guarantee of performing transactions, SQL databases are still the better bet.

Most of the NoSQL solutions sacrifice ACID compliance for performance and scalability.





**Reasons to use SQL database**

We need to ensure ACID compliance

Your data is structured and unchanging.

If your business is not experiencing massive growth that would require more servers and if you're only working with data that is consistent,

**The reason use SQL database**

Storing large volumes of data that often have little to no structure












