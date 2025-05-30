# Nosql



---

NoSQL is a collection of data items represented in a key-value store, document store, wide column store, or a graph database.



Data is denormalized, and joins are generally done in the application code. Most NoSQL stores lack true ACID transactions and favor [eventual consistency](https://github.com/donnemartin/system-design-primer/blob/master/README.md#eventual-consistency).



Eventual consistency

After a write, reads will eventually see it (typically within milliseconds). Data is replicated asynchronously.

~~This approach is seen in systems such as DNS and email. Eventual consistency works well in highly available systems.~~

Strong consistency

After a write, reads will see it. Data is replicated synchronously.

This approach is seen in file systems and RDBMSes. Strong consistency works well in systems that need transactions.






