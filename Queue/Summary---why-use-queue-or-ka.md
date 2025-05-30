# Summary - why use queue or kafka 



---

easy to scale. you really can have more consumers, each consumer can subscribe same or different topic



Queue is buffer between the consumer and producer, producer may sent the huge request to the consumer layer, the consumer service is not ready to handle that load, producer can write the message to the queue and consumer can read the message from the queue at certain rate. We call load leveling, we cannot control the rate producer insert the message to the queue but we can control the rate or frequency consumer read the message from the queue



Reliability, queue can be a backup storage even the consumer is not working or dead, we still can save the message in the queue and pick up by another consumer



ensure all the message were processed in order
