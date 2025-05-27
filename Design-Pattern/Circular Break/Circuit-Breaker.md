
---

If we have any service consumer, that would be either a website or an application. Talking remotely to other service providers: We have to do two things: we have to be available and responsible.

Availability means I can even get the service. When I get the service, I get time to respond.

The circuit breaker pattern is very similar to the circuit breaker in your home. In other words, does it allow all electricity to flow through your plans and lights? When there is an issue, when something bad happens, the break immediately happens. It breaks all the electricity into the home so that it doesn't burn out the site.

The circuit breaker in software works the same way between applications and services. Set a circuit breaker. The circuit break continue to monitor the microservices. If everything is healthy in the microservice, the circuit remains closed. When the request was made, for instance, for that trade, the "service consumer "checked that circuit breaker first and said: Breaker, are you closed. Yes. Now I know the microservices are responsive. However, I can make the call when bad things happen to good people. That means microservices become non-responsive. The breaker constantly monitors the microservice and immediately notifies and opens it.

Suppose I want to send a request. I checked the breaker and said: Are you close? No, I am open. An error occurs. I discovered through the circuit breaker that the microservice is not responsive in 10 million seconds. Every client finds out through the circuit breaker that there is an error in the microservice. That circuit breaker still monitors the microservice to say Are you up and running, are you better. Now the microservice has become responsive. The circuit breaker is closed, allowing all the transactions to go through.

There are 3 basic types of circuit breakers you might try. 

1. Simple heartbeat. It is not very useful for the responsive message, but it is helpful for the ability. They do not work well for the circuit breaker. From the responsive standpoint, it is that heartbeat response, but I do not know the "place trade function."

2. Send a "transaction or fake transaction, a fake number, a fake bank account or credit number. The circuit break will continue to call the "place trade function" with the fake number. So they can determine the response for the partial function

	The problem with this method is that all the systems, including all downstream systems and the report system, need to know that the number I just posted is fake and cannot be processed. The second problem with that transaction is that we only test a general one-pass through that "place trade." We do not really get an accurate picture of what happens with that( "place trade " function), a certain function.

3. Really user monitor. Which is a better way to determine the health and responsiveness of any given operation: to monitor the health transaction? There is no time out with a circuit breaker; other uses include "trend analysis."

I will go through some numbers, and send for that response time for "trade ", watch what happens

So the break is closed. We assume the response time we got is 
2.1 
1.6 
1.8 
2.5 
1.5 
1.9 
2.7 
3.6 
4.8 
5.2 open

Did you see what happened? The trend was significant, going up. Most users do a standard deviation analysis because they all go up and down. But that trend was way past 33 standard deviations. In that case, the circular open means that it is not allowing requests to that microservice.

So, I need to determine how the microservice will become healthy again. That case is called the open state. That means I don't allow any transactions except for a couple.

The state is called an open state. That means I don't all

A circuit breaker acts as a proxy

Half-Open: A limited number of requests from the application are allowed to pass through and invoke the operation. If these requests are successful, it's assumed that the fault previously causing the failure has been fixed, and the circuit breaker switches to the Closed state (the failure counter is reset). If any request fails, the circuit breaker assumes that the fault is still present, so it reverts to the Open state and restarts the timeout timer to give the system a further period to recover from the failure.

It can help maintain the system's response time by quickly rejecting a request for an operation that's likely to fail rather than waiting for the operation to time out.


**circuit breaker 2**

Your application uses a circuit breaker connected to the service, and it calls the closed state. If the circuit is closed, you can connect to the outside service. You have an API out there. Your circuit breaker said I am in the closed state. I can connect. Then the client sent out the information and got the response.

Now you have made another request. Somehow, the current state is still close. But there is a timeout, and you cannot reach the service anymore. (Some bad thing happened on the service)

What happens is that the state has now changed to open. There is no bridge; the circuit breaker is closed like a bridge connected open right now, and both can go through. Now, it is the open state, rather than calling the service. You responded right away. You know it is closed, you don't need to connect again for now. After a specific time, try to connect the service again. They said OK, the connection runs through, just somehow this thing happened. The connection goes down. It comes back up automatically

And now it's back to the closed state.

When it is half open, it goes back to the closed state. Then you can actually go to communication again.

**When use:**

When you want to prevent the application from trying to invoke a remote service to access a resource if this operation is highly likely to fail

Give the user a better experience, not just loading, give the user a better response to the application

**When not use**

You don't want a circular break pattern for the hash table when you need to handle access to a local private resource application, such as an in-memory data structure.