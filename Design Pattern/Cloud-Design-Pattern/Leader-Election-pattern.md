# Leader Election pattern



---

For example:



In a cloud-based system that implements horizontal scaling, multiple instances of the same task could be running at the same time with each instance serving a different user.

If these instances write to a shared resource, it's necessary to coordinate their actions to prevent each instance from overwriting the changes made by the others.

If the tasks are performing individual elements of a complex calculation in parallel, the results need to be aggregated when they all complete.

The task instances are all peers, so there isn't a natural leader that can act as the coordinator or aggregator.



Solution





There are several strategies for electing a leader among a set of tasks in a distributed environment, including:



Selecting the task instance with the lowest-ranked instance or process ID.

Racing to acquire a shared, distributed mutex. The first task instance that acquires the mutex is the leader.

However, the system must ensure that, if the leader terminates or becomes disconnected from the rest of the system, the mutex is released to allow another task instance to become the leader.



Implementing one of the common leader election algorithms such as the Bully Algorithm or the Ring Algorithm. These algorithms assume that each candidate in the election has a unique ID, and that it can communicate with the other candidates reliably.







It must be possible to detect when the leader has failed or has become otherwise unavailable (such as due to a communications failure).

Using a shared, distributed mutex introduces a dependency on the external service that provides the mutex. The service constitutes a single point of failure. If it becomes unavailable for any reason, the system won't be able to elect a leader.







This pattern might not be useful if:







The Distributed Mutex project in the LeaderElection solution (a sample that demonstrates this pattern is available on GitHub) shows how to use a lease on an Azure Storage blob to provide a mechanism for implementing a shared, distributed mutex.



This mutex can be used to elect a leader among a group of role instances in an Azure cloud service.

The first role instance to acquire the lease is elected the leader, and remains the leader until it releases the lease or isn't able to renew the lease.

Other role instances can continue to monitor the blob lease in case the leader is no longer available.



A blob lease is an exclusive write lock over a blob. A single blob can be the subject of only one lease at any point in time.

A role instance can request a lease over a specified blob, and it'll be granted the lease if no other role instance holds a lease over the same blob. Otherwise the request will throw an exception.



To avoid a faulted role instance retaining the lease indefinitely, specify a lifetime for the lease. When this expires, the lease becomes available. However, while a role instance holds the lease it can request that the lease is renewed, and it'll be granted the lease for a further period of time. The role instance can continually repeat this process if it wants to retain the lease. For more information on how to lease a blob, see Lease Blob (REST API).




