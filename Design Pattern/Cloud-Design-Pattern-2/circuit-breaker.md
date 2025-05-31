# circuit breaker



---

if we have any sort of service consumer, that would be either web site or application. Talking remotely to other servicer: we have to do 2 things, we have do availability and responsibility.

Availability is mean can I even get the service. When I got the service, I get time to response.

The circuit breaker pattern is very similar with the circuit breaker in your home. In other words, it remains does it allow all electricity flow your plans, lights. When there isaissue. When something bad happened. The break immediate happen. It break all electricity out into that home.so that protectdoesn't burn out the site.



The circuit break in software work exact the same way between application and service . set a circuit breaker. The circuit break continue monitor the microservices. If everything health in the microservice, the circuit remain close. When the request made for instance for that trade, the "service consumer " check that circuit break first and said: breaker are you close. Yes. Now I know the microservices is responsive. Now in can to make the call. However, when the bad things happen to the good people. That is mean microservice become no responsive. The breaker constantly monitors the microservice and immediate notify and open

If I want to send a request. I check the breaker and say are you close. No I am open. And error occurs. I found out through the circuit breaker that microservice is not responsive in 10 million second. Every client find out though the circuit breaker that there is an error in microservice. That circuit break sill monitor the microservice to say are you up on running are you better. Now the microservice is now become responsive. The circuit break is close and allowing all the transaction trough.

There are 3 basic type of circuit break you might try. 1 simpleheart beat. It is not very useful for responsive message. It is useful for ability. There are not work well for the circuit break. For the responsive stand point, is that heart beat response but I have no knowledge about the "place trade function "



2.send a "" transaction or faker transaction, a fake number, fake bank account or credit number. The circuit break will continue call the "place trade function" with the fake number. So they can determine that responsive for the partial function

The problem of this way is all the system including alldown streamsystem and report system need know the number I just post in is fake and cannot processed. That lead second problem with that transaction that is we only test general one pass through that "place trade". We not really get the true picture of what happen of that( "place trade " function) certain function



3. really user monitor. Which is better way to determine that health and responsive of any give operation: to actual monitor the heathy transaction. There is no time out with circuit break other use "trend analysis "



I will go through some number of which number of send for that response time for "trade " watch what happen

So the break is closed. We assume the responsive time we got is 2.1 1.6 1.82.51.5 1.9 2. 7 3.6 4.8 5.2 open

Did you see what happen. The trend was significant going up. Most user stand deviation analysis because all up set down? But that tread were way pass 33 standard deviation. In that case , the circular open, that it is no allowing request to that microservice.

So I need determine that how the microservice become healthy again. That case is called half open state. That is mean I don't allow any transaction throw except a couple

The state is called half open state. That is means I don't all





A circuit breaker acts as a proxy



Half-Open: A limited number of requests from the application are allowed to pass through and invoke the operation. If these requests are successful, it's assumed that the fault that was previously causing the failure has been fixed and the circuit breaker switches to the Closed state (the failure counter is reset). If any request fails, the circuit breaker assumes that the fault is still present so it reverts back to the Open state and restarts the timeout timer to give the system a further period of time to recover from the failure.



It can help to maintain the response time of the system by quickly rejecting a request for an operation that's likely to fail, rather than waiting for the operation to time out,



**circuit breaker 2**

your application use circuit break connect to the service and it call close state. If the circuit is close you are able to connect the outside service. You have a API out there. You circuit break said I am close state. I can connect. then the client sent out the information and get the response





now you made another request. somehow the current state is still close. but there is a timeout and you cannot reach the service anymore. ( some bad thing happen on the service)



what happen the state now change to open. There is no bridge, circuit break closed like bridge connected open right now and both can go through. now is open state rather calling the service. you response right the way. you know it is closed, you don't need connect again for the time being. After certain amount of time you may think try to connect the service again. They said OK the connection run through just somehow this thing happen. connection goes down. it comes back up automatically



and now it back to the close state.



when it is like half open state then it back to the close state. Then you can actually go to communication again.



when use:

when you want to prevent application from trying to invoke remote service of access resource if this operation is highly likely to failure



give user a better experience not just loading loading give a user better response of the application



when not use

when you need handle access to local private resource application such as in memory data structure



you don't want to circular break pattern for hash table. .
