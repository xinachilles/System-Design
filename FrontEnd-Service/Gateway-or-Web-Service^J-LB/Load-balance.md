# Load balance 



---

we can now handle all of the requests from our clients and it would have balanced way





but of course this is assuming that all of our clients are going to be issuing requests to our servers in a balanced way, our requests are evenly distributed amongst our servers



load balancer is going to be a server that sits in between a set of clients and a set of servers



by the way you can think of load balancers as types of reverse proxies most of the time, because load balancers sit between clients and servers, and they typically act on behalf of the servers, which is exactly what a reverse proxy does





you might have another load balancer in between your servers and your databases



when you're dealing with the website you might even have load balancing at the DNS later





In the video there's something called DNS round-robin, which is basically a type of load balancing that happens at the DNS layer ,where a single domain name gets multiple IP addresses ,and when a DNS queries made get the IP address of that domain name ,the multiple IP addresses that are associated with that domain name ,or going to be returned in a load balanced way



how load balancers actually select servers to send traffic to ,there are a lot of different ways that they can do that, the first one that you might random distribution



2. round-robin is basically a method that goes through all of your servers in one order, so maybe here it goes from top to bottom, and then it goes back to the top, so here if your clients were issuing requests it would all go to your load balancer, and your load balancer would then say okay I'm redirecting the first request to the first server, the second request of the second server, that's pretty nice because you sort of guarantee that you will be evenly Distributing traffic across your servers slightly



more complex type of round-robin strategy that you can have your load balancer follow is what's called a weighted round-robin where you basically Place weights on specific service,



meaning you would still follow the order of going to the first one first ,and the second one and the third one but you might put a weight on let's say the second server which would mean that when your load balancer is redirecting the traffic to a second server, it's going to redirect a couple more request ,a little bit more traffic, than it redirects to the other servers, and you could imagine that you might want to do that it would say one of your servers is more powerful than the others.





another way for a load balancer to select servers is based on performance or based on load this strategy is a little bit more involved basically ,your load balancer performs health check on your servers ,to know how much traffic a server is handling at any given time ,how long server is taking to respond to traffic ,how many resources of servers using, and based on that it'll redirect traffic accordingly



if this server here at the bottom is not handling request well, maybe it's doing really poorly is getting overloaded somehow it's going to start redirecting request not to the server



another very common server selection strategy is service selection strategy we're basically when your load balancer gets requests from clients it's going to hash the IP address of the clients, and depending on the value of the hash, it will redirect the traffic accordingly



it can be really useful if you've got for instance cashing going on in your servers ,you could imagine that it's in your system here you are caching the results of requests in your servers, it would be very helpful to have requests from a specific client always be redirected to the server in which the response of that particular clients request has been cashed, and that can be

accomplished through this IP based load balancing





IP base load balancing can help you maximize your cache hits, every time that this particular client is who's a request you know that it'll be rerouted to this server and you might have cache that client's request





Final method of service selection for low downstairs that can be very useful is a path based service election strategy, with this strategy and load balancer basically distributes request to servers according to the path of the request, we distribute some requests to different servers according to their path, basically what this means is that all requests related to running code on algo exper t or going to be redirected to a specific server or a specific set of servers all requests related to payments on auto expert meaning purchasing the product or going to be redirected to another server or to another set of servers





and this can be really useful because what it means is that if we ever want to deploy a big change to a service like for instance imagine that we want to deploy a big change to our code execution engine this deployment would only affect the service handling the code execution engine requests, and all the requests that are related to payments on auto expert with still get routed to their own specific set of servers



so far code execution servers start dying for whatever reason all of our other services on the platform or unaffected




