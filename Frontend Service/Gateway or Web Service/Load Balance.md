
---
## Load Balancer Summary

- **Purpose**: A load balancer **sits between clients and servers** to distribute incoming requests evenly across multiple servers, improving reliability and performance.
you might have another load balancer in between your servers and your databases

- **Relation to Reverse Proxy**: Load balancers often act as reverse proxies, handling requests on behalf of servers.
- **Types of Load Balancing**:
  - **DNS Round-Robin**: At the DNS layer, a domain name maps to multiple IPs, and DNS responses rotate these IPs to distribute traffic.
  - **Random Distribution**: Requests are sent to randomly selected servers.
  - **Round-Robin**: Requests are distributed sequentially among servers, ensuring even traffic.
  - **Weighted Round-Robin**: Servers are assigned weights; more powerful servers receive more requests.
  - **Performance/Load-Based**: The load balancer monitors server health and load, directing traffic to less busy or healthier servers.
  - **IP Hashing**: Requests from the same client IP are always routed to the same server, maximizing cache hits for that client.
  - **Path-Based Routing**: Requests are routed based on the request path (e.g., code execution vs. payments), allowing for service isolation and targeted deployments.
- **Benefits**:
  - Ensures even distribution of traffic.
  - Improves system reliability and scalability.
  - Enables targeted routing for specific services, which helps with maintenance and scaling.
  
---



