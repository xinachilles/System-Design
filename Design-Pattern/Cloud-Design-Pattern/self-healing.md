# self healing

Created: 2018-10-01 23:19:39 -0600

Modified: 2018-10-02 09:11:46 -0600

---

Design Principle --

Build redundancy into your application, to avoid having single points of failure



Place multiple VMs behind a load balancer.

Replicate databases.



Enable geo-replication. creates secondary readable replicas of your data in one or more secondary regions.



Partition for availability. Database partitioning is often used to improve scalability, but it can also improve availability.



Deploy to more than one region. For the highest availability, deploy the application to more than one region.




