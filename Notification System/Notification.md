
---

1. Send the message to a message queue
2. The work pool is another side, and a worker will pick the message and call the notification API, and transfer the message to one of the notification services
3. The user will create a long polling HTTP connection to the notification service, and this service will pull the message to that user
4. The worker will go back to the message queue to send this message send successfully






