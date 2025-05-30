# Ambassador and bulkhead



---

The ambassador provides routing, circuit breaking, and logging. It calls the remote service and then returns the response to the client application:

It is often used with existing code libraries, or other applications that are difficult to modify to add these features, in order to extend their networking capabilities.

we also can update the proxy without disturbing the existing code.

Deploy the proxy on the same host environment





Bulkhead pattern

Isolate elements of an application into pools so that if one fails, the others will continue to function.


