# Loaded queue

Created: 2018-12-05 00:52:46 -0600

Modified: 2019-01-04 17:32:08 -0600

---

Use a queue that acts as a buffer between a task and a service to smooth heavy loads that can cause the service to fail or the task to time out.



The queue decouples the tasks from the service, and the service can handle the messages



It's necessary to implement application logic that controls the rate at which services handle messages.



Message queues are a one-way communication mechanism. If a task expects a reply from a service, it might be necessary to implement a mechanism that the service can use to send a response.

For more information, see the Asynchronous Messaging Primer.









Example

A Microsoft Azure web role stores data using a separate storage service.





This figure highlights a service being overwhelmed by a large number of concurrent requests from instances of a web role.







To resolve this, you can use a queue to level the load between the web role instances and the storage service.





However, the storage service is designed to accept synchronous requests and can't be easily modified to read messages and manage throughput.



You can introduce a worker role to act as a proxy service that receives requests from the queue and forwards them to the storage service.



The application logic in the worker role can control the rate at which it passes requests to the storage service to prevent the storage service from being overwhelmed.

This figure illustrates using a queue and a worker role to level the load between instances of the web role and the service.
