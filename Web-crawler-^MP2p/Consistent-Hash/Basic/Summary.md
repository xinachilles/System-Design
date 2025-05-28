# Summary

Created: 2021-01-17 17:28:53 -0600

Modified: 2021-01-25 18:50:48 -0600

---

DHT algorithm or chord algorithm to find which node should be response for the practical URL



So we assume there n service in the network, we take address of a node of a peer so the IP address and in practice not just the IP address but also the port number and get 160 bit value ( sha1 will produce 160 bit value)





All the url and file we also need hash and get 160 bit value as well



The node i will response for the url i



Node 1 node 2 node 3 node 4 and come back around to node 1 again so we'll visualize as some ring or some circle ordered by ID



Treat the hash space as a ring ,visually we can think of them in placed in a circle



if there's a node if there's no node that has that idea in our network then the resources stored at the next node with it in the ring then over the next highest ID.



[What happens if new node joins a network]{.mark}



we need to update the keys managed by the successor node, so if node 13 join to the network, it will take portion resource from 14



[similar when we leave a network]{.mark} , [a node leaving the network we need to update the keys for the successor]{.mark}



[Each node will have a routing table]{.mark}



[how cord actually works is the node table maintain more than one entry and the scheme is to maintain entries for example two nodes which are 1 ,2 ,4 ,8 positions away so we double the distance from our node, if we have 2^160 node, one node will have 160 entries from 2^0 to 2^159 which across half of network]{.mark}



[The table also has a key space, it map a node id to the key]{.mark}



we're searching, each node support manage some [key space]{.mark} some

interval of keys, the first node is assumed to maintain the key 1 the second node which will be two positions away should manage keys 2 & 3



For downloading, we also need a table to remember for the file management, for example, when we download a file and its hash id 16

We will go to 16 node to check this file is down or not







The problem for the consist hash is not distributed the load evenly

Load distribution



Least connection policy, least busy server get the request



~~Another solution is we can hash the request twice and compared the result of hash the request once, those 2 are different, choose a server have less load~~





![• Compute the average # of requests per server • Add some overhead, say 25% • Round up to get the max # of requests per server • If the chosen server is below the max, send the request there • Otherwise, go around the hash ring (forwarding) until one is below the max ](../../../media/Web-crawler-^MP2p-Consistent-Hash-Basic-Summary-image1.png){width="5.0in" height="1.09375in"}



Get the max request of each server == average of request for each server

Add

















