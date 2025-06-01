# Summary 



---

**Function**

If user type something, system should suggest top 10 terms starting with base on user has typed.

need ask the top 10 is for 1 hours 1 day or one year

we assume there will be 500M active user and each user search 6 times and each search user need type 6 characters to get the result they want

**CAP: 500 * 6 * 6 /86400 =200k**

Latency is impart for us, search typehead need compete with the typing spread

need to have low latency

**~~Consistency need ask during the interview.~~**

Not really important. If 2 people see different top 5 suggestions which are on the same scale of popularity, its not the end of the world. I, as a product owner, am happy as long as the results become eventually consistent.

How import the available

Very important. If search typeahead is not available, the site would still keep working. However, it will lead to a much degraded experience.

**Different with other solution, we can talk about the tire first**

A trie is a tree-like data structure used to store word and each node stores a character of the word. We also need store the top suggestion regarding each word in the node

For example, if we need to store 'cap, cat, caption, captain, capital' in the trie, it would look like:

We can optimize our storage by storing only references of the terminal nodes rather than storing the entire word.

To find the suggested term we've to traverse back from the termain node to root use the parents reference.

**What the size of the tire**

**Storage Estimation:** If on the average each query consists of 6 characters and assuming we need 2 bytes to store a character, we will need 12 bytes to store an average query. So total storage we will need:

18 billion = 500 * 6(search 6 times) * ( each search user need type 6 characters)

18 Billion * 12bytes => 216GB

we can assume that only 1% of these will be unique

so the storage every day we need is 1.8GB

We can expect some growth in this data every day, but we should also be removing some terms that are not searched anymore. If we assume we have 2% new queries every day and if we are maintaining our index for last one year, total storage we should expect:

3GB + (0.02 * 3 GB * 365 days) => 25 GB

since we store the word in the tire data structure, the really storage should be less than this number

**How to collect the data**

We have a data collection service, we the new updates come in, We can log them and also track their frequencies.

**to reduce the log file size**

if we don`t want to show a term which is search more the 1000.

We don`t need log every query. I can do sampling and log every 1000th query.

We can have a Map reduce setup to process all the logging data periodically, say every hour. These MR jobs will calculate frequencies of all searched terms in the past hour.

How to update the tire

**there are two solution** we don`t need store the frequency, we just need rebuilt the tire in another service offline as we don't want our read queries to be blocked by update trie requests.

we can have a master-slave configuration for each trie server. We can update slave while the master is serving traffic. Once the update is complete, we can make the slave our new master. We can later update our old master, which can then start serving traffic too.

the issue of this solution is wait the new service set up and we still need use the old data and may not 100 % represent the top 10 words in last week

**other solution is**: We will also need to store the frequency and update the frequency

We can update only difference in frequencies rather than recounting all search terms from scratch.

If we're keeping count of all the terms searched in last 10 days,

we'll need to subtract the counts from the time period no longer included and add the counts for the new time period being included.

~~We can add and subtract frequencies based on [Exponential Moving Average (EMA)](https://en.wikipedia.org/wiki/Moving_average#Exponential_moving_average) of each term. In EMA, we give more weight to the new data. It's also known as the exponentially weighted moving average.~~

[for example, assume we count of all the term for last 10 days - t1 ,the weight of t1 will be 1/3 and the old data t2 in the node will be 2/3 so the total count will be t1 *1/3 + t2*2/3]{.mark}

After inserting a new term in the trie, we'll go to the terminal node of the phrase and increase its frequency. Since we're storing the top 10 words in each node.

the parents will store the top 10 hot words who prefix is like aaa....

[not just insert the new node, also need update the frequency of his parents]{.mark} node if the current query's frequency is high enough to be a part of the top 10. If so, we insert this new term and remove the term with the lowest frequency.

**How can we remove a term from the trie?** Let's say we've to remove a term from the trie, because of some legal issue or hate or piracy etc. We can completely remove such terms from the trie when the regular update happens, meanwhile, we can add a filtering layer on each server, which will remove any such term before sending them to users.

**How to store trie in a file so that we can rebuild our trie easily - this will be needed when a machine restarts?**

partition

**a. Range Based Partitioning:** What if we store our phrases in separate partitions based on their first letter. So we save all the terms starting with letter 'A' in one partition and those that start with letter 'B' into another partition and so on.

We can even combine certain less frequently occurring letters into one database partition.

[**Partition based on the hash of the term:** Each term will be passed to a hash function, which will generate a server number and we will store the term on that server.]{.mark}

This will make our term distribution random and minimizing hotspots.

To find typeahead suggestions for a term, we have to ask all servers and then aggregate the results. We have to use consistent hashing for fault tolerance and load distribution.

use this way, it is difficult store the top 10 word in each node, we need store a frequency (count of search)in the end of the node, for example, user search cat 100 times and caption 500 times, we can store this number with the last character of the phrase.

we have to ask all servers and then aggregate the results. We have to use consistent hashing for fault tolerance and load distribution.


