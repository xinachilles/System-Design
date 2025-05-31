# Bulkhead patterns



---

~~Let say we have couple of service~~

~~Now other request is coming in. let say that case, we have one more payment request and one more car rent request. Two other thread will be sign to do it. Since the payment service is still very slow. The third payment third is still waiting for the payment service response and other one handle the car rent request will free very quickly.~~

~~So right now, we have 3 thread are awaiting for payment service response.~~

After a while, if we have mix request coming since some program service is very slow and we cannot get the request very quickly.

At one point time, All the thread are awaiting the service to do its job.

Did that mean all the service cannot service any more request.



To insure the one service is not affect other service. We have to roughly divided the connection pool by number of service we have. For example, if we have 200 threads on the connection pool and we have two service to be called.

We want to ensure all the request for payment service. Max only number of this thread and not more than that . only 100 request for that payment service.

If there one more request for payment. Since we already reach the max. we should not allow it call the payment service. We will block that request from calling the payment service and instead we should return the default response back to user. If we do that the thread will quickly become free and we will not have the same problem before. The whole connection pool will not be exhaust become the payment service.

Setting the maximum thread hold for number of certain request. Tracking the number of Current Request to partical service and according allowing deny the request to exteral services

Ensure we still can keep on service which for the other services

Bulkhead is used such situation if there is a problem is one service or independency doesn't not affect or ability to allow the request of any other indecency.

Insure we still able to service other service. We need mateine a counter for counting the number of different type request. First we have to check the current number of request is less the threadhold we sent.



We also can send a max wait time if there are 150 calls max being made. How much amount of time that request wait before sending a new request?
