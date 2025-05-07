# Gatekeeper pattern

Created: 2018-11-08 14:30:42 -0600

Modified: 2019-12-16 12:32:46 -0600

---

Context and problem

If client connect the application service directly ,user can gain unrestrained access to sensitive information and other services.

Solution

You can achieve this by using a facade or a dedicated task that interacts with clients and then hands off the request



~~This pattern acts like a firewall in a typical network topography.~~ It allows the gatekeeper to examine requests and make a decision about whether to pass the request on to the trusted host (sometimes called the keymaster) that performs the required tasks.



This decision typically requires the gatekeeper to validate and sanitize the request content before passing it on to the trusted host.

Issues and considerations

Consider the following points when deciding how to implement this pattern:



- Typically this means running the gatekeeper and the trusted host in separate hosted services or virtual machines.



- The gatekeeper shouldn't perform any processing related to the application or services, or access any data.



- Use a secure communication channel (HTTPS, SSL, or TLS) between the gatekeeper and the trusted hosts or tasks





- The gatekeeper instance could be a single point of failure.

When to use this pattern

This pattern is useful for:

- Applications that handle sensitive information, expose services that must have a high degree of protection from malicious attacks, or perform mission-critical operations that shouldn't be disrupted.



- Distributed applications where it's necessary to perform request validation separately from the main tasks, or [to centralize this validation to simplify maintenance and administration.]{.mark}
