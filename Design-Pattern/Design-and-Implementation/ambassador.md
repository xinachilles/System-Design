# ambassador 



---

<https://www.youtube.com/watch?v=q8xGBZwjRO4&t=4s>



ambassador pattern is out of process use of proxy. it was may used for performance common client connect task like ~~monitor, log, routing, security and resiliency pattern like bulkhead pattern retry pattern~~

Like check circuit breaker state

Add some tracing information to the request header, log the request, security











if you want this-- connect task to use in the latency system which the application is quite complex to modify. Suppose we have latency application we want to add some kind of client connectivity task like monitor security or some resiliency pattern like retry pattern, circular break. we don't want to modify the latency application. This case we will create a separate application to handle this task.



How it's work, the latency application will call the ambassador application and

the ambassador application will call the remote service and all performance this task, when receive the response from the remote service, then sent back to the latency application. Both latency application and ambassador application on host on the same plate form or same environment.



Most benefit is all the client connectivity task can be offer to separate application which can be managed indepently



the second benefit if we are not disrupting latency complex application. so all the client connectivity task manner separately without latency system.



other benefit is the ambassador system can be shared by multiple process for example we have 2 or 3 application want to make a better system. we can handle all the request and mage accordingly



concerns



we add one more layer between remove service and client or latency system. I will add some latency. there are some latency overhead



when to use

if we have the latency application and application is different to modify and you want to add some client connectivity task. or if we have application and we want to separate the responsibility like client activity task. or we want to share the responsity client activity cross multiple process that we can use this



not use

we don't want latency between the process














