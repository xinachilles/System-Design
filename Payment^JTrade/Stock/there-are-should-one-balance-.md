# there are should one balance table and one trade table

Created: 2020-11-27 16:59:05 -0600

Modified: 2020-12-03 01:51:54 -0600

---

there are should one balance table and one trade table



the customer call the "place trade API" and the API service will push the message to the pub/sub service and the pub/sub service has multiple topic , the topic is shared by customer key



there are multiple worker wait on other side and we need make sure a practical work is for

practical customer, if one customer has multiple place trace request, we need make sure we can process in some order



[we may need cluster rather than only single one would be if we want to really optimized for that high availability]{.mark}



[we would also be guaranteed at least once delivery through these messaging system]{.mark} so we would know that we would never lose a specific trade





the worker will pass a call back the exchange service and after get the feedback, update the trade service and update blance take and go the queue to check if that customer has any other trade



we may need another service for heath check periodically query, all the trades in the trade table that our place for in progress and check if they're not being handled ,










