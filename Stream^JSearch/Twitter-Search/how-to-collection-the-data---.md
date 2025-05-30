# how to collection the data - Wormhole ( like kafka)



---

Collecting the Data



we can use pub sub way to collect the data



Publisher will identifies the new writes and reads updates from the data storage system transaction logs and push them to interested applications or subscribers that include News Feed, index servers, Graph Search and many others.





Data storage systems are typically geo-replicated with a single master, multiple slaves topology. Wormhole publishers running on the slave can simply read new updates off the local transaction log and provide updates to local subscribers.



publisher has a tailor and the tailor read the transaction log once and transfer those log to key value pairs and give it to publisher.



Publisher copy it and sent it off and the data is processed on the application side.



Publishers periodically ask the application: hi application, are you done with the message number 10. Just make sure the application received the data. if publisher receiver this message and it will move the application maker to 10 th



There are some application marker in the traction log that indicates the position of the last received and acknowledged update of the flow by the subscriber.



otherwise We need replay some past data for those application data base on the marker







In case of error happened , the publisher needs to restart sending updates from a stored datamarker or recovery tailor





It is a tradeoff between IO and latency,



Base on the IO and latency, we can just one recovery tailor to start reading the minimum of both application marker which replay the data for both applications but blue application need wait to the green application catch up or we can have two recovery tailor which read the data in different place ,put more IO load but low latency





When the recovery tailor and main tailor read the data at the same point. Publisher realize those factor that We back the good the state again



The tailor read the transaction log once is good for IO. It is IO efficiency





How to handle the failure



On the publisher side, Wormhole provides multiple-copy reliable delivery where it allows applications to configure a primary source and many secondary sources they can receive updates from. If the primary publisher (which generally is the publisher in the local region) fails, Wormhole can seamlessly start sending updates from one of the secondary publishers.

On the subscriber side, an application that has registered for updates can also fail. Wormhole publishers periodically store for each registered application the position in the transaction logs of the most recent update it has received and acknowledged. On an application failure, Wormhole finds where to start sending updates from based on its bookkeeping, maps that to the correct update in the datastore's Data Model and System Architecture. A dataset is a collection of related data, for example, user generated

Wormhole maintain a separate publish-subscribe system pre shard



The publishers finds all interested applications and corresponding subscribers via a ZooKeeper-based configuration system.

Subscribers receive the stream of updates for every shard, which we call a flow.


