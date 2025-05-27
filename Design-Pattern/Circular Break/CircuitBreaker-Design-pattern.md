<https://itnext.io/understand-circuitbreaker-design-pattern-with-simple-practical-example-92a752615b42>

Suppose we have two servers, A and B, in A has two APIs

**/data** that depends on service B

**/data2** does not depend on any external service

Without a circuit design pattern

For Service B, the API is returning a 5-second delayed response to a request for the first 5 minutes.

**Why do we need a circuit breaker?**

In case we have service B down, service A should still try to recover from this and try to do one of the following:

1. **Custom fallback:** Try to get the same data from some other source. If not possible, use its own cache value.

2. **Fail fast:** If service A knows that service B is down, there is no point waiting for the timeout and consuming its own resources. It should return ASAP, "knowing" that service B is down

3. **Don't crash:** As we saw in this case, service A should not have crashed.

4. **Heal automatically:** Periodically check if service B is working again.

5. **Other APIs should work:** All other APIs should continue to work.

6. **Closed:** The application's request is routed to the operation. The proxy maintains a count of recent failures, and if the call to the operation is unsuccessful, it increments this count.

If the number of recent failures exceeds a specified threshold within a given time period, the proxy is placed into the **Open** state.

At this point the proxy starts a timeout timer, and when this timer expires the proxy is placed into the **Half-Open** state.

The purpose of the timeout timer is to give the system time to fix the problem that caused the failure before allowing the application to try to perform the operation again.

**Open:** The request from the application fails immediately, and an exception is returned to the application.

**Half-Open:** A limited number of requests from the application are allowed to pass through and invoke the operation.

If these requests are successful, it's assumed that the fault was previously causing the failure has been fixed and the circuit breaker switches to the Closed state (the failure counter is reset).

If any request fails, the circuit breaker assumes that the fault is still present so it reverts back to the **Open** state and restarts the timeout timer to give the system a further period of time to recover from the failure.



