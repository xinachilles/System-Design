# Pillars of software quality



---

A successful cloud application will focus on these five pillars of software quality: Scalability, availability, resiliency, management, and security.





ScalabilityThe ability of a system to handle increased load.

AvailabilityThe proportion of time that a system is functional and working.

ResiliencyThe ability of a system to recover from failures and continue to function.

ManagementOperations processes that keep a system running in production.

SecurityProtecting applications and data from threats.





Scalability

Scalability is the ability of a system to handle increased load. There are two main ways that an application can scale.

Vertical scaling (scaling up) means increasing the capacity of a resource, for example by using a larger VM size.

Horizontal scaling (scaling out) is adding new instances of a resource, such as VMs or database replicas.



Horizontal scaling has significant advantages over vertical scaling:



True cloud scale. Applications can be designed to run on hundreds or even thousands of nodes, reaching scales that are not possible on a single node.

Horizontal scale is elastic. You can add more instances if load increases, or remove them during quieter periods.

Scaling out can be triggered automatically, either on a schedule or in response to changes in load.

Scaling out may be cheaper than scaling up. Running several small VMs can cost less than a single large VM.

Horizontal scaling can also improve resiliency, by adding redundancy. If an instance goes down, the application keeps running.





An advantage of vertical scaling is that you can do it without making any changes to the application. But at some point you'll hit a limit, where you can't scale any up any more. At that point, any further scaling must be horizontal.
