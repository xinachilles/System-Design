# Presence 

Created: 2021-01-11 18:27:37 -0600

Modified: 2021-01-26 19:16:39 -0600

---

1.  When member "Alice" opens LinkedIn on her mobile device or a browser, a persistent connection is established with the Frontend connection manager The existence of this connection is a clear indication that Alice is currently online.


1.  Alice will check the presence platform first



2.  Secondly, Alice may be interested in viewing whether Bob is currently online. For that, the [Real-time Platform allows Alice to subscribe to a topic for Bob's presence status.]{.mark}



3.  When bob is on line, it will connect the "Frontend connection manager" service



4.  The "Frontend connection manager" to be able to detect the existence of the persistent connection to determine when a member comes online.



5.  The Real-time Platform, in turn, starts emitting periodic heartbeats with the member's ID with a fixed duration of d seconds between successive heartbeats.



6.  When the platform received the heartbeat:





- For each received heartbeat corresponding to a member ID, it checks whether we have an unexpired entry for that member ID in our distributed K/V store for Presence.

Member ID -> last heartBeat Time stamp



- If there is no entry or if previous entries have expired,
  - We conclude that the member just came online.
  - We publish an online event on the presence status topic for that member on the Real-time Platform to distribute that event to the member's connections.



- We add an entry to the store with an [expiry duration which is slightly larger than d seconds (d + ε) to keep the record alive till at least the next heartbeat.]{.mark}



- If an unexpired entry exists, the member is already online and we simply update the lastHeartbeatTimestamp for that member ID to the latest value.



7.  we also built what we called a [delayed trigger]{.mark} for each member that is currently online. we are able to start a timer that will fire later to allow us to check whether the heartbeat stored for that member has expired or not. Since heartbeats expire in d + ε seconds, we need the timer to fire a little after that or in d + 2ε seconds



Thus, during the process heartbeat step, we create this delayed trigger if it doesn't yet exist for the member. If it already exists, we simply reset it to fire in another d + 2ε seconds. When the delayed trigger for an online member fires, we check whether the member's heartbeat has expired in our K/V store. If it has, we publish an offline event on the presence status topic for that member on the Real-time Platform to distribute the fact that the member has gone offline to the member's connections.




