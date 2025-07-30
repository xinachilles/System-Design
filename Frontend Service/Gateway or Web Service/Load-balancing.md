## Load Balancing Summary

- **Placement**: Load balancers can be used between:
  - Clients and application servers
  - Application servers and database servers
  - Application servers and cache servers

- **Basic Strategies**:
  - **Round Robin**: Evenly distributes requests among servers. Simple, but doesn’t account for server load.
  - **Intelligent Load Balancing**: Monitors server health/load and adjusts traffic accordingly, preventing requests from being sent to overloaded or dead servers.

- **Additional Benefits**:
  - **SSL Termination**: Decrypts incoming requests and encrypts responses, offloading this work from backend servers.
  - **Session Persistence (Sticky Sessions)**: Ensures a client’s requests are routed to the same server, using cookies or session data.

- **Types of Load Balancers**:
  - **Layer 4 (Transport Layer)**: Distributes requests based on IP addresses and ports. Fast and resource-efficient, but less flexible.
  - **Layer 7 (Application Layer)**: Distributes requests based on application data (headers, URLs, cookies). More flexible, but may require more resources.

- **Key Tradeoff**: Layer 4 is faster and lighter, while Layer 7 offers more routing flexibility.

---


<https://github.com/donnemartin/system-design-primer/blob/master/README.md#load-balancer>

![1) 2) 3) Pick a worker to forward request Random Round robin Least busy • Sticky session / cookies By request parameters Wait for its response Forward the response to client Client Dispatcher Worker Worker Worker ](../../media/FrontEnd-Service-Gateway-or-Web-Service^J-LB-Load-balancing-image1.jpg)