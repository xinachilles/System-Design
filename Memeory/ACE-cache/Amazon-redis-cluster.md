# Amazon redis cluster 

Created: 2022-02-13 16:30:07 -0600

Modified: 2022-02-18 12:07:10 -0600

---

![Redis Cluster-mode Availability zone 1 ( CW Metric: Currltems ) Distribution Equal I Custom Partitioned by Shard Shard 1 Slot O- Shard 2 Slot to Shard 3 Slot . to . Shard 4 Slot ... to 16383 Elastic Load Balancing Auto Scaling group Cluster Map -3 Configuration Endpoint Zero Downtime Scaling Availability zone 2 Amazon EC2 Private subnet Redis node Redis node Clients use hash value for a key CRCI 6(key) mod 16384 Am EC2 -o o Private subnet Redis node Redis node ](../../media/Memeory-ACE-cache-Amazon-redis-cluster-image1.png){width="5.0in" height="2.5347222222222223in"}







A configuration endpoint as we discussed which will hold reference to all of the available shards.



As well as all of the available replicas in your cluster so whereas before you had one shard with up to five replicas you might have 10 charts with each shard having its own to replicas.

