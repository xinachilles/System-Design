
1. 
2. Send the message to a message queue
3. The work pool is another side, and a worker will pick the message and call the notification API, and transfer the message to one of the notification services
4. The user will create a long polling HTTP connection to the notification service, and this service will pull the message to that user
5. The worker will go back to the message queue to send this message send successfully






