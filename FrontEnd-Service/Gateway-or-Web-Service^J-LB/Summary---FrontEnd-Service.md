# Summary - FrontEnd Service 



---

![FrontEnd Service A lightweight web service Stateless service deployed across several data centers Request validation Authentication/Authorization TLS (SSL) termination Server-side encryption Caching Actions Rate limiting (throttling) Request dispatching Request deduplication Usage data collection ](../../media/FrontEnd-Service-Gateway-or-Web-Service^J-LB-Summary---FrontEnd-Service-image1.png)





FrontEnd is a lightweight web service responsible for: request validation, authentication and authorization, SSL termination, server-side encryption, caching, throttling, request dispatching and deduplucation, usage data collection.





When request lands on the host, the first component that picks it up is a Reverse Proxy.



[Reverse proxy is a lightweight server responsible for several things]{.mark}.



Such as SSL termination, when requests that come over HTTPS are decrypted and passed further in unencrypted form.



At the same time proxy is responsible for encrypting responses while sending them back to clients.



Second responsibility is compression (for example with gzip), when proxy compresses

responses before returning them back to clients.



This way we reduce the amount of bandwidth required for data transfer.





Next proxy feature we may use is to properly handle FrontEnd service slowness.



We may return HTTP 503 status code (whichis service unavailable) if FrontEnd service

becomes slow or completely unavailable.



[Reverse proxy then passes request to the FrontEnd web service.]{.mark}



~~We know that for every message that is published, FrontEnd service needs to call Metadata service~~



~~to get information about the message topic.~~



To minimize number of calls to Metadata service, FrontEnd may use local cache.



We may use some well-known cache implementations

(like Google Guava) or create our own implementation of LRU cache (least recently used).



[FrontEnd service also writes a bunch of different logs.]{.mark}



What information is this?



We definitely need to log information about service health, write down exceptions that happen in the service. We also need to emit metrics.



This is just a key-value data that may be later aggregated and used to monitor service

health and gather statistics.



For example, number of requests, faults, calls latency, we will need all this information

for system monitoring. Also, we may need to write information that may be used for audit, for example log who and when made requests to a specific API in the system.



[Important to understand here, is that FrontEnd service is responsible for writing log data.]{.mark}



But the actual log data processing is managed by other components, usually called agents.



Agents are responsible for data aggregation and transferring logs to other system, for post processing and storage.



This separation of responsibilities is what helps to make FrontEnd service simpler, faster and more robust.

