# Mongo DB

Created: 2019-05-29 23:05:04 -0600

Modified: 2019-06-03 23:32:21 -0600

---

Mongo DB is used to handle the document data model like jason

Or log post documents



Mongo DB database contains collection and collection contains ducument



Mongo DB is consistency and partition tolerance it chose favor consistency over availability



M has single master and single primary database service you have to talk all the time you have to ensure the consistency



M has many secondary database in the different data center maintain copy of primary database so the secondary data get the replicated via the operation log



the secondary can be selected to the new primary if the primary goes down



application needs know who is the primary node



sharding the Mongo DB, we can have multiple replica set each replica set responsible for some range of data



application server will talk to MongOS and MongOS will talk to the config server, config server will store how thing partitioned and figure out which replica set we can talk to get the information that i want



MongOS also can re bala nce the data




