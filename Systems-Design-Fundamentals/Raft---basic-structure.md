# Raft - basic structure

Created: 2020-11-23 12:50:59 -0600

Modified: 2020-11-23 13:06:58 -0600

---

rafe application level having the application code, key value store, table of key and value, on the top and on the bottom is the raft library.. send and recevie RPC, cooperate wit each other replica





raft has a log of operations to record the operations

client just consider the whole replica cluster is one service and the request like get value or update will forward to the leader and leader will repose the request to client



the leader will forward the request to all replica and [until all the replicas are a majority the replicas get this new operation into their logs.]{.mark} and then when its leader knows that all of the replicas of a copy of this,



only then as a leader raft sent a notification back up to the key value (application layer) saying aha that operation you sent me it's been now committed into all the replicas ,and so it's safely replicated at this point it's okay to execute that operation ,





then raft notifies the leader now the leader actually execute the operationthen finally sends the reply back to the client





yeah and so in addition when operations finally committed each of the replicas sends the operation up each of the raft library layer sends the operation up to the local application layer, in the local application layer applies that operation to its state





[time diagram of how the messages work]{.mark}



the client sending the original request to server 1, after that server 1 raft layer sends an append entries RPC to each of the two replicas ,this is just an ordinary, I'll say a put request

this is append entries requests ,the server is now waiting for replies and the server's from other replicas as soon as replies from a majority arrive back including the leader itself, so in a system with only three about because the leader only has two, wait for one other

replica to respond positively to an append entries, as soon as it assembles positive responses from a majority, the leader execute a command ~~figures out what the~~

~~answer is like~~ and sends the reply back to the client,



when the leader execute it and reply yes to the client



once the server 1, or leader realizes that a request is committed it then needs to tell the other replicas to execute the operation and apply the command to their state
























