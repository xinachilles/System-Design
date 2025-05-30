# Storm 



---

[SOUND]

what is Storm? So Storm was developed as a real time streaming system with a set of goals.



storm can gartueen, they want to make sure every single event is eventually processed by the system.



They want to have horizontal scalability. What we mean here is that if the system. Isn't enough for the streaming loads coming in. We can just add machines of similar type to the cluster.



They want to provide fault-tolerance so

that if one of the machines or some of the machines fail, or if there's

a network failure, the system can kill those notes launch news then sent somewhere else and continue processing.







They want to make sure they system

doesn't have a single point of failure or hotspot to make the scaling issue. Therefore, they want to make sure there's no intermediate message brokers. All the notes can talk to

each other if need be.





Storm ahs a single master note. It's called nimbus. That oversees the flow of information in the system, and basically manages the other

components in the system.



And the communication typically happens through a small zookeeper cluster.



Each of the cluster notes has a supervisor daemon running on it, which basically gets its assignments from Nimbus, and starts running a user task and

can send a result, and by result I mean the job progress or the status of the job, back to Nimbus.



Supervisors talk to Nimbus through a Zookeeper cluster and make sure cluster coordination





Streams are a sequence of tuples. Now, what do I mean by tuple?



A tuples typically can have a key and a value like many of the other big data or

cloud systems that we talked about.



Now stream is a sequence of these

tuple going from somewhere to another.





Second concept here is a spout. Spout is a source.



The third component, are the bolts.



A Bolt is a component that gets a certain amount of input and produces output.



in bolts, you can program your functions, filters, joins, aggregations,

database lookups, basically anything that you want to do with your program,

you write them as bolts.



topology is really your program that is built out of bolts, spouts, and processes streams.





it can have one bolt. That is really running on 1,000 different machines.



When a tuple is emitted,



First one is shuffle grouping. or field grouping. What field grouping does is that it takes a consistent hashing function



assign those tuple to machine run the bolt


