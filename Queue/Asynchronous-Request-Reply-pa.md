# Asynchronous Request-Reply pattern



---

<https://youtu.be/3bxAm3XIFmk>



Asynchronous Request-Reply pattern

~~with event driven architecture. We know we can leverage message to replay to any kind of architecture style. It goes composability more to different architecture partner and style.~~

The message queue or request reply pattern. It is very powerful but it's an asynchronous protocol and question usually become how do I get responses in the kind of asynchronous protocol.

This message channel really consist two queues. There is a request queue which we send request to different service or application and a reply queue which that service and application sends us the responds. Those both are asynchronous. I like call thissudo- synchronized message.

Because there is what happen. When I sent a request to service, let say get the name of the customer. After I sent it I no long have to wait. I can free to do whatever I want to do. As the service is processing to get the name. I am able to do some other processing then I am waiting. I do a blocking wait on the reply to and service components, on the right side, when he done to retrieve the name and sent to me. Now I have a name. That essentialityhave the request pattern works.

[How this work]{.mark}

We implement the request -pattern with correlation id. We do have a sender and receiver. Let saying we are getting the first name of customer, those first name already there. Those are ours. Now what I want to do is sendinga message say give me the first name of customer ABC. Notice what I sent the message to request queue has a id "124 ". I sent this request to the queue than what I do, I do a blocking wait on the relay queue. Using some call message selector. It also could be message filter waiting the message when that correlation id -- CID equals to my message id which is number "124". Notice before there are still a couple of message is waiting be retrieved. Notice the correlation id 120, 122. There are not mine. Those are still sitting on the queue waiting person retrieve.

Now, the receiver receive the message from me, id 124 and goes does looking up the name. then they sent the correlation id equal to my original message id in other words number 124 there. Then sent the response. Notice when the sending happen. There get another unique message id ,That case is 857 and correlation id is my origin message ID. It goes to my reply queue. Notice I am doing the block wait waiting on the receive queue for that specific correlation id. There is message 123 with the message id from receiver 857 that I got the specific first name from the customer.

There are other way you can do request-reply message is that with temperate queue. A lot simpler in there the sender and receiver is no response queue initial. I want to send a message instead of using correlation id. I use something call temporary message queue, in other word reply to in the message head itself. I am saying reply to temperate queue t1.so the message broker itself will create the temperate queue.

Here is thing no one know it exists except inside that message.Notice that reply queue is give me the name of the queue which usually isuuid, for example. So I just do a blocking wait on that temple queue because it's only mine. the receiver received that message, they do the look up for name. they sent the responsive. Now I don't need the message for select. I just simply get the responsive. Once I receive the message, message broker will remove the queue





Third solution to this problem is to use HTTP polling.

The client application makes a synchronous call to the API, triggering a long-running operation on the backend

1.The client sends a request and receives an HTTP 202 (Accepted) response.(let the clientknows that API accepted the request and started working)

With the HTTP 202 header,



2.The client sends an HTTP GET request to the status endpoint. The work is still pending, so this call also returns HTTP 202.

3.At some point, client sent another HTTP Get request and the work is complete and the status endpoint returns 302 (Found) redirecting to the resource.

4.The client fetches the resource at the specified URL.

An HTTP 202 response should indicate the location and frequency that the client should poll for the response. It should have the following additional headers:
