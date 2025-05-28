# CircuitBreaker Design pattern

Created: 2018-10-31 00:13:06 -0600

Modified: 2019-06-21 08:58:11 -0600

---

<https://itnext.io/understand-circuitbreaker-design-pattern-with-simple-practical-example-92a752615b42>



suppose we have two servers A and B, in A has two APIs



/data which depends on serviceB

/data2 does not depend on any external service



Without circuit design pattern



for Service B The API is returning a 5 second delayed response to a request for the first 5 minutes.



Why we need a circuit breaker

In case we have serviceB down, serviceA should still try to recover from this and try to do one of the followings:



Custom fallback: Try to get the same data from some other source. If not possible, use its own cache value.



Fail fast: If serviceA knows that serviceB is down, there is no point waiting for the timeout and consuming its own resources. It should return ASAP "knowing" that serviceB is down



Don't crash: As we saw in this case, serviceA should not have crashed.



Heal automatic: Periodically check if serviceB is working again.



Other APIs should work: All other APIs should continue to work.







Closed: The request from the application is routed to the operation. The proxy maintains a count of the number of recent failures, and if the call to the operation is unsuccessful the proxy increments this count.



If the number of recent failures exceeds a specified threshold within a given time period, the proxy is placed into the Open state.

At this point the proxy starts a timeout timer, and when this timer expires the proxy is placed into the Half-Open state.



The purpose of the timeout timer is to give the system time to fix the problem that caused the failure before allowing the application to try to perform the operation again.



Open: The request from the application fails immediately and an exception is returned to the application.



Half-Open: A limited number of requests from the application are allowed to pass through and invoke the operation.

If these requests are successful, it's assumed that the fault that was previously causing the failure has been fixed and the circuit breaker switches to the Closed state (the failure counter is reset).

If any request fails, the circuit breaker assumes that the fault is still present so it reverts back to the Open state and restarts the timeout timer to give the system a further period of time to recover from the failure.

![](../../media/Design-Pattern-Resiliency-CircuitBreaker-Design-pattern-image1.png){width="5.375in" height="0.4236111111111111in"}![](../../media/Design-Pattern-Resiliency-CircuitBreaker-Design-pattern-image2.png){width="4.479166666666667in" height="0.4375in"}![](../../media/Design-Pattern-Resiliency-CircuitBreaker-Design-pattern-image3.png){width="4.833333333333333in" height="0.4375in"}



