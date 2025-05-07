# Raft - Split brain

Created: 2020-11-23 12:30:00 -0600

Modified: 2020-11-23 12:40:16 -0600

---

1.  what is split brain



suppose we have 2 client and 2 service, one is master and one is replica



we cannot ask client talk to both service or wait both service response







the troubling scenario is both service are alive but because the network issue the client c1 only can talk to service 1 and client c2 only can talk to service 2



client1 is gonna send a test and set request to server 1, server 1 will you know set it state to one and return the previous value of zero to client one and



but this replica service 2 still of zero



the client 2 initial a talk and



in it all right so now if client2 who've also sends a test and set request to both service and only get service 2's reply



we also cannot wait 2 service's reply. it is not fault-tolerant or if you just wait for one and you will not get correct answer



it's often called split brain


