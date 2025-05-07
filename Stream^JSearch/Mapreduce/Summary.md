# Summary 

Created: 2018-01-30 01:03:48 -0600

Modified: 2018-01-30 01:14:26 -0600

---

MapReduce is a programming model to parallel process the large data set on the computer cluster.



Generally, a map function, emitting (key, value) pairs for each input and a reduce function that processes the list of values with the key, coming from all the mappers

**For the word count:**

the input data will be the database log, the map reduce frame will chops it up and pass to lot of map function, map function will emit the key value pair, key will be each word and value is one



before start running the reduce function, frame also do some group processing which group the same key together.



it says, okay, there were five different keys created by my map functions, for example,

So function says hey key 1 had a value coming from map one, and a value coming from map five. That's still fine. It groups all of these and creates those lists.



it says okay, now it's your turn again. It starts calling reduced functions,

So it will say I will instantiate five different invocations of function reduce.



for the reduce function the input will be one key and a list of values and output will be the sum of those value
