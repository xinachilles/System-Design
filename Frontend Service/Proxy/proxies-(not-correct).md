## Forward Proxy and Reverse Proxy

- **Proxy** is a general term for a server that acts as an intermediary in network communications. The term is often used loosely, but there are two main types:
  
### 1. Forward Proxy
  - Sits between clients and servers, acting on behalf of the client.
  - Clients send requests to the forward proxy, which then forwards them to the destination server.
  - The server sees the request as coming from the proxy, not the original client (the proxy’s IP replaces the client’s IP).
  - Common uses: hiding client identity, bypassing restrictions, or accessing restricted content (e.g., VPNs).

  ### 2. Reverse Proxy
  - Sits between clients and servers, acting on behalf of the server.
  - Clients send requests to the reverse proxy, which then forwards them to the appropriate backend server.
  - The client sees the reverse proxy as the server (DNS points to the proxy’s IP).
  - Common uses: filtering requests, handling authentication, gathering metrics, caching content, and load balancing.

---

**Key Difference:**  
- Forward proxies serve the client’s interests (client-side), while reverse proxies serve the server’s interests (server-side).
