# Problem 

Created: 2021-01-03 16:23:17 -0600

Modified: 2021-01-03 17:58:29 -0600

---

Overview

1.  View my schedule for next week, next month or next year(read)
2.  View friend's schedule(read)

Event:

1.  Participants and status
2.  Meeting rooms
3.  Link to zoom

Create event:

1.  Set visibility(write)
2.  Add guest(write)
3.  Add zoom(write)
4.  Add meeting room - show meeting room availability(write after read)
5.  Set recurring frequency / schedule(write)

Requirements:

1.  Create and send an event
2.  Receive an event
3.  DownLoad all event
4.  Add room
5.  Visibility







System requirements:

6.  Reliable
7.  Consistent





Traffic:

1.  1B , create 1 event / day, receive 3 event/ day
2.  Storage: 1 B 10 ^ 9 * 1 * 10 BK * 365 day * 5 = 15Pb; (300 ~ 1k)
3.  QPS: write 10 ^ 9 * 1 / (24 * 3600) = 2 k
4.  Read : 2 k * 10 = 20 k/ s; (comment)



Solution:

write after read

DB locker in the read and write only one user can access the DB.



What's the lock granularity, what's you are going to lock there

Lock





Boolean true;

Scott -> room1 ->08/04:7pm

Rui -> room1 -> 08/04:7pm



1.  [How to book the room]{.mark}


1.  Queue: scott -> rui - > jim
    1.  Scott -> long pause (network, process pause, GC, 1min) -> aborted?
        1.  Timeout at 1s -> 86400 req / day

Set<Room ID> process ; room1; 1K rooms;





Partition based on room Id: Book room A Req2 US -> server1(crashed)

Book room A Req1 China -> server1(crashed) → clock drift 2 hours slower

Server 1 crash -> request asked be retried again



Room booking request --routing to server partitioned by room id--> server accepted request → in memory queue → claim a lock to the database which has room A data → if it is your turn, then you could claim your room.







Pessimistic locking





In memory req 1 -> room A [lock]

Server crash: A release

Room A[lock] → timeout(db request) database crash ->





Write skew

Correctness : ????

- App Server 1 (crashed)
  - Request 1 -> Scott -> room1 ->08/04:7pm -> database (read then write)
- App server 1'
  - Request 2-> Rui -> room1 ->08/04:7pm -> database(read then write)

=====timeline view ==========================

i ---- 1 ----- | txn 1 ------------------------------ commit |

Ii -----1'--- | txn2 ----------------------commit|

=========================================

Efficiency: Database lock ->

- 2 phase locking: -- write / read blocked
- Material conflicts: - write/read not blocking
- [Lock on index:]{.mark} write / read not blocking
  - Lock on room A -- DIY
  - Lock on timeslot

Read requests ->





Add gues lock

Add room non- blocking 乐观锁

Add lock for room

Add lock for
