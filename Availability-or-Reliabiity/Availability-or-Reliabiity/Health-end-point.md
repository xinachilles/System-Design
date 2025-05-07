# Health end point

Created: 2019-11-03 23:14:36 -0600

Modified: 2020-09-08 09:37:55 -0600

---

~~When you deploy the system, build the system you run the test, when you have deployed. How did you know it's healthy? You don't want the user or sendaemail the service is not responsive~~

There are two elements. A agent and application. Application expose the endpoint

So the agent can make a request and the application will run some check function and return the status code to the agent.

On the application side, checking maybe depend on the resource, data storage, database. Agent on the other hand will evaluate the response and see the update is in good shape. Response code200, 500. How much response time it took because increase number of request time will indicate some error happen.

~~Agent also will check other part of system. CDN, security, DNS look up table make sure every Conner of system function fine~~



~~Security the endpoint~~

~~How to configure security for the monitoring endpoints to protect them from public access, :~~

~~•Secure the endpoint by requiring authentication. You can do this by using an authentication security key in the request header or by passing credentials with the request,~~

~~oUse an obscure or hidden endpoint. For example, expose the endpoint on a different IP address to that used by the default application URL, configure the endpoint on a nonstandard HTTP port, and/or use a complex path to the test page. You can usually specify additional endpoint addresses and ports in the application configuration, and add entries for these endpoints to the DNS server if required to avoid having to specify the IP address directly.~~

~~oExpose a method on an endpoint that accepts a parameter such as a key value or an operation mode value. Depending on the value supplied for this parameter, when a request is received the code can perform a specific test or set of tests, or return a 404 (Not Found) error if the parameter value isn't recognized. The recognized parameter values could be set in the application configuration.~~


