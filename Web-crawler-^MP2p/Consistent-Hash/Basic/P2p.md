# P2p



---







gossip protocol



1.  each node has a neighbor list, when the node received the message, it will forward that message to the node on the list
2.  the protocol contain a fan number, the max message each node will send out
3.  ttl, time to leave, if the initial number is 3, after we send the message to another node, this number is decreased by 1, = 2 if the ttl is 0, we stop transfer the message
4.  need a visited array to remember which node has been visited







ACKs and NACKs might implode, meaning that there might be very large numbers of ACKs and NACKs.



For instance, for multicast where the initial dissemination was not very successful and was received at very few,

uh, nodes in the group.



To avoid this, the SRM protocol adds random delays at the receiver.

So receivers when they realize they need to send out a NACK, they don't send out the NACK immediately.

They wait for a little bit of time and then they send it out. If they need to send a NACK multiple times,



then they might use an exponential backoff, meaning they wait for a period of time that doubles every time they wait.

also, the messages that are sent out might also be subject to exponential backoff.




