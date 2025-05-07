# Competing Consumers pattern 

Created: 2018-11-14 01:01:45 -0600

Modified: 2020-09-09 11:38:28 -0600

---

Competing Consumers pattern

Use a message queue to implement the communication channel between the application and the instances of the consumer service.

The application posts requests in the form of messages to the queue, and the consumer service instances receive messages from the queue and process them.





This solution has the following benefits:



It provides a load-leveled system that can handle wide variations in the volume of requests sent by application instances.

The queue acts as a buffer between the application instances and the consumer service instances.

(described by the Queue-based Load Leveling pattern)

Handling a message that requires some long-running processing doesn't prevent other messages from being handled concurrently by other instances of the consumer service.



Reliability: If all the consumers are unresponsive or overloaded, the system can still continue working and no messages would be lost.



It's scalable. The system can dynamically increase or decrease the number of instances of the consumer service as the volume of messages fluctuates.



Resiliency: If the consumer fails mid-task, the message is not lost, but rather returned to the queue, to be picked up by another consumer.



Issues and considerations

Message ordering. we can not guaramteed the message order



One-Way: As we discussed in the Queue-Based Load Leveling Pattern, this Pattern is fully decoupled, so if the service generates a result,

it must be passed back to the application logic and must be stored in a location that is accessible to both.



Scaling the messaging system. In a large-scale solution, a single message queue could be overwhelmed by the number of messages and become a bottleneck in the system.



Ensuring reliability of the messaging system.

Poison Messages: any message that a service cannot process, may be returned to the queue crashing other services and creating and infinite loop processing these messages.



When to use this pattern

Use this pattern when:



The workload for an application is divided into tasks that can run asynchronously.

Tasks are independent and can run in parallel.

The volume of work is highly variable, requiring a scalable solution.

The solution must provide high availability, and must be resilient if the processing for a task fails.

This pattern might not be useful when:



It's not easy to separate the application workload into discrete tasks, or there's a high degree of dependence between tasks.

Tasks must be performed synchronously, and the application logic must wait for a task to complete before continuing.

Tasks must be performed in a specific sequence.



Example

Azure provides storage queues and Service Bus queues that can act as a mechanism for implementing this pattern.

The application logic can post messages to a queue, and consumers implemented as tasks in one or more roles can retrieve messages from this queue and process them.

For resiliency, a Service Bus queue enables a consumer to use PeekLock mode when it retrieves a message from the queue. This mode doesn't actually remove the message,

but simply hides it from other consumers. The original consumer can delete the message when it's finished processing it.

If the consumer fails, the peek lock will time out and the message will become visible again, allowing another consumer to retrieve it.
