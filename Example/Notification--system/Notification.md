
---

1.  send the message to a message queue
2.  the work pool is another side and a work will pick the message and call the notification API and transfer the message to one of notification service
3.  the user will create long polling http connection to the notification service and this service will pull the message to that user
4.  the worker will go back to the message queue to make this message send successfully






