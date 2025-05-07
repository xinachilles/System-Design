# Raft- leader election

Created: 2020-11-23 13:17:42 -0600

Modified: 2020-11-23 13:23:22 -0600

---

[leader election]{.mark}



raft uses these" term numbers" in order to sort of disambiguate which leader we're talking about.



How do the leaders get created, in the first place, every raft server keeps this election

timer, it has recorded that says well if that time occurs ,I'm going to do something and the something that it does is that if an entire leader election period expires without the server having heard any message from the current leader, then the raft servers sort of assumes probably that the current leader is dead and starts an election, so we have this election timer and if it expires, we start an election and what it means to start an election is basically that you increment the term.



every term has it must most one leader. and each replica only can vote one candidate in one term



leader need get yes vote from majority service



the new leader will sent heartbeat to all replica i am the new leader for term x, this heartbeat will reset all replica's election time





raft also random these election timers so the way to think of it and the everybody received the last append entries ,and then maybe the server died let's just assume the server send out a last heartbeat and then died. well all of the followers re-set their election timers when they received at the same time. because they probably all receive this append enters at the same time. They all reset their election timers for some point in the future ,But they chose different random times in the future which the timers are gonna go off



Assuming they picked different random, numbers one of them is first and the, other one is second. Assuming this gap is big enough, the one that's first it's election time will go off first before the other ones election timer and if we're close were not unlucky it'll have time to send out a full round of vote requests and get answers from everybody who everybody's alive before the second election timer goes off from any other server




