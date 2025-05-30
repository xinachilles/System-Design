# proxies (not correct)



---

we're going to be covering two primary types a proxies r[everse proxies, and foward proxies]{.mark}, in the industry it turns out that people use the term proxy pretty Loosely and when they use the term proxy they often mean foward proxy,



jump into the definition of proxies we'll start with a forward proxy to put it very simply a forward proxy is a server that sits in between a client or a set of clients and another server or a set of other servers



but more specifically a forward proxy is a server that acts on behalf of the client or clients



you can almost think of the [forward proxy as being on the client's team or on the client side of an interaction between a client and a server]{.mark}, in practice what this means is that if a client like this client here wants to communicate with a Server ,Like This Server here assuming the forward proxy has been configured correctly, by the client when the client is going to issue a request to the server, instead of going directly to the server, its first going to go to the proxy to the forward proxy ,which then is going to forward the request of a server so you can think of it as the client does a request that's meant to go to the server but first goes to the forward proxy, Somerset the client who sang Hey foward proxy communicate with the server on my behalf please, and in the forward proxy will forward the request to the server ,then the server gets the request, but the server gets it from the forward proxy, the server doesn't get the requested directly from the client, so when the server response it's going to give its response to the proxy, and the forward proxy is going to give the response to the client







The Source IP address is going to be P here is the IP address of the store forward proxy in other words The Source IP address from the initial request which would have been a from the client can get removed and replaced by P in this new request





the forward proxy or VPN is there basically hide client identity



and by the way this means that a client might be able to access restricted servers that it's otherwise not supposed to access because the forward proxy might hide who this client really is



[reverse proxies act on behalf of a server in an interaction between a client and a server]{.mark}, so the best way to think about it here is that if a client wants to interact with the server so the client wants to send a request to the server if the reverse proxy has been configured properly by the server, or by The Entity that owns the server, then when the client is going to Issue request of a server, the request is actually going to go to the reverse proxy







so here DNS would return the IP address are because the reverse proxy IP address





for instance you might configure your reverse proxy such that it can filter out requests that you want to ignore, maybe for your system you don't want your servers to ever deal with certain kinds of requests ,so you can have your reverse proxy and filter them out, you can maybe have a reverse proxy take care of login for your system, if you want to watch stuff you want to gather metrics



if you want to cash stuff maybe you want to cache certain things like, a html Pages you might be able to do that at the reverse proxy later and that way your server doesn't get bothered too much perhaps one of the best use cases of a reverse proxy is using a reverse proxy as a load balancer










