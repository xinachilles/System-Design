# Ajax polling, Http long polling and web socket (tcp connection) 

Created: 2018-01-28 17:38:21 -0600

Modified: 2021-01-12 00:04:57 -0600

---

**web socket**, use tcp connection connect the client and service side, they can exchange the information on both side, it is two way conversation,



~~the cost is cheap and easy to implement~~





**Http long poll**



it is http connection, client start send a initial http request and wait for the response



service will hold the response until the update is available, if the update is available, send a response to client



then client will send a new long pull request to service



the connection [has a timeout]{.mark} so client will send a new long pull connection after the time out







**Ajax polling**



client open a connection and request data from service

service will send response even there a no updating (empty message)

client will send the request to service at regular intervals








