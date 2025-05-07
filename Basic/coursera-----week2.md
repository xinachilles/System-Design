# coursera --- week2 

Created: 2020-07-16 16:57:02 -0600

Modified: 2020-07-16 17:21:00 -0600

---

Multicast problem





in the network underneath. So one of the simplest way of doing multicast, is a centralized approach. You have the sender, it has a list of recipients, it simply goes through

those recipients, in say, a for- or while-loop and it sends each

of these recipients either, a UDP or a TCP packet that contains the information. Uh the multicast message itself. UDP stands for User Datagram Protocol. It is a connectionless and unreliable way of transporting messages, but it's cheap and it's low overhead. TCP is

Transmission Control Protocol, which is a connection oriented way and a more reliable way

of sending



uh, messages. You could choose

either one of these. This is, of course,

the simplest implementation. Now, uh, one of the problems

with this implementation is, uh, when it comes

to fault tolerance. If the sender fails when it's

halfway through its "4" loop, essentially,

only half the receivers, uh, would have received

the multicast. Also, the overhead

on the sender is very high. Imagine a group where you have thousands of, uh, nodes in the group and, uh, in that particular case, uh, the sender has, uh, to go through, uh, has to send

a lot of messages. And this also increases

the latency, uh, the average time for a receiver to receive a multicast, could be as high as, um, O(N), which is, linear in the size

of the group itself. So to address that, the Tree-based Multicast Protocols have been developed, essentially, all of these, uh, uh, protocols develop a spanning tree among the, uh, nodes or the processes in the group. Uh, these include,

network level protocols, such as, IP multicast, where the spanning tree's among the routers and switches, in the underlying network, but also application

level multicast protocols, such as, SRM, RMTP, TRAM, TNTP, and a whole bunch of others. Uh, so, m, first of all,

this is attractive because, uh, the, uh, if you build a tree,

uh, well enough, if it is say, a balanced tree, then with N,

uh, nodes in your group, the height of the tree

is O(log(N)) and this means that the latency for a message to reach, any of the nodes

in the group is O(log(N)) as opposed to centralized, where the latency might have been, as high as, O(N) Also, the overhead

on each member in the group, both the sender, uh, as well as, any

of the receivers, is constant, especially if, the number

of children is constant. Uh, essentially, you recieve one copy of the multicast message and you send it out- send out as many copies

as you have children. However, one of the problems

here is that you need to, um, uh, set up

and maintain the tree. When, uh, load failures happen,

uh, the uh, um, nodes in the system might actually not receive multicast, for a while. For instance, if, uh, if this node at-at the leaf fails, then, uh, that's the only node that's affected, when that node rejoins the tree, uh, it will start

to recieve multicast. However, if this node that's

closer to the root fails, then essentially,

you have a situation where, none of its descendants

will receive any of the multicasts

that are sent, uh, before, uh, this part

of the tree is repaired. Perhaps it is replaced

by another node or it recovers and rejoins the tree

at that particular position. So, uh, because, as you know, failures are the norm

rather than the exception, you're always

going to have situations where nodes are failing

all the time and you have to spend some amount of bandwidth and resources in co-continuously repairing the tree, all the time. So, a little bit more on the

Tree-based Multicast Protocols. Essentially, these, uh,

build a spanning tree among the processes

of the multicast group. They use a spanning tree

to disseminate the multicast. They might prefer IP multicast

where its available-available to, uh, do this

initial dissemination. Thereafter, they use

either acknowledgements, positive acknowledgements

or negative acknowledgements, uh, to repair the multicasts that are not received by the, uh, receivers. Positive acknowledgements

are often called as ACKs and negative acknowledgements are called as NACKs, Uh, for instance, the Scalable

Reliable Multicast protocol, the SRM, uses NACKs. Essentially, um, uh, when

a receiver has not received, uh, multicast messages that it is expecting or for a while, it sends a repair request

up towards, uh, the root of the tree, and when these repair requests are received, uh, nodes closer to the root

of the tree send, uh, the latest multicasts

that they have or the missing multicasts as far as the receiver is concerned. One of the issues with

tree-based protocols is that, uh, the, uh, ACKs

and NACKs might implode, meaning that there might be very large numbers of ACKs and NACKs. For instance, for multicast

where the initial dissemination was not very successful

and was received at very few, uh, nodes in the group. To avoid this, the SRM protocol

adds random delays, uh, at the receiver. So receivers when they realize

they need to send out a NACK, they don't send out

the NACK immediately. They wait for a little bit of

time and then they send it out. Uh, If they need to send

a NACK multiple times, uh, then they might use

an exponential backoff, meaning that, uh, they, uh,

wait for, uh, a period of time that doubles every time

they wait. Uh, also, the messages

that are sent out might also be subject

to exponential backoff. The RMTP, the Reliable Multicast

Transport Protocol, uh, ha-is similar,

but it's slightly different. Instead of using

negative acknowledgements, it uses

positive acknowledgements. So, uh, the receivers

periodically send a digest or a collection

of acknowledgements for all the multicasts

that they received so far, uh, and, uh, if any of the messages are missing from this, uh, from these ACKs, then those messages are sent downwards towards the receivers. Now once again, you can have

ACK storms here and, uh, to avoid these

ACK storms, uh, there are certain receivers marked as designated receivers and, uh, the ACKs are sent only to these designated receivers and then these designated receivers forward, uh, the, uh, corresponding multicast messages down the tree. However, there have been

studies that have, uh, analyzed the scalability

of these protocols and even, in spite,

of the random delays and en-and exponential backoff, uh, both these, uh, uh, clas-uh, both these types of protocols, both, uh, NACK based

and ACK based protocols still suffer from, uh, uh, uh, a large number of NACKs

and ACKs that are sent out and the number of these

NACKs and ACKs typically grows

linearly with the, uh, size of the group itself. So these protocols are not as

scalable as they were intended, uh, to be. So this motivates

the development, um, or the introduction

of our gossip based or epidemic based protocol.


