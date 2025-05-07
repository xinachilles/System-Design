# Map reduce for the typehead:

Created: 2018-01-09 14:49:16 -0600

Modified: 2021-01-16 19:03:40 -0600

---

MapReduce is a programming model that allows parallel processing of large datasets on a computer cluster.

Abstractly, a map function, emitting (key, value) pairs for each input split, and a reduce function that processes the list of values with the key, coming from all the mappers

**For the word count:**

the input data, all of it, it chops it up, passes each one to a map function, ( a separate map function, map function one, map function two, map function three. It calls map function for each of these

map function , input is from log database, value will be the log data, we don`t need key in here.

And I'm writing pseudo code here of course. So I can say for each long entry , you can emit

an intermediate key value pair. The key I use here is the word. And the value is one.

Before it's start running your reduced program, reduced function, framework also does some sort of processing and group values which have the same key together



it says, okay, there were five different keys created by my map functions, for example,



So function says hey key 1 had a value coming from map one, and a value coming from map five. That's still fine. It groups all of these and creates those lists. So now your key one will have value eight coming from map one, value nine coming from map five,



it says okay, now it's your turn again. It starts calling reduced functions,



In our examples we had five different keys created by the user. So it will say I will instantiate five different invocations of function reduce. For function one I will pass key one, and this list of values. And reduce function two, key two and its own associated list of values, and so on and so forth.

Okay so now what I do in my reduce function for my word count problem, I get one key and a list of values.

In map function, key would be a word. One word. Value will be the list of value from mappers

Finally, it will emit a key value pairs and key still the word and value will be the sum of those values


