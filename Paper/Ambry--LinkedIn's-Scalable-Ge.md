# Ambry: LinkedIn's Scalable Geo-Distributed Object Store

Created: 2024-01-30 17:04:11 -0600

Modified: 2024-01-30 17:14:25 -0600

---

Summary

Ambry is a decentralized geo-distributed object store developed by LinkedIn to efficiently handle large immutable data, or "blobs," in a scalable and load-balanced manner, addressing challenges such as varying blob sizes, rapid request growth, and workload variability.

Facts

Challenges with Blob Handling:

- Blobs range in size from tens of KBs to several GBs, requiring efficient storage and retrieval.
- LinkedIn serves over 800 million put and get operations per day, highlighting the need for scalability and low overhead.
- Workload variability and cluster expansions can lead to unbalanced load and degraded system performance.



- For instance, the hierarchical directory structure and rich metadata are an overkill for a blob store and impose unnecessary additional overhead



- Ambry's Design Goals:
- Low Latency and High Throughput:Utilizes techniques like OS caching, zero-copy data reading, and parallel chunk retrieval to serve requests efficiently.
- Geo-Distributed Operation:Replicates blobs across datacenters for high durability and availability, with asynchronous replication and proxy requests for low latency.
- Scalability:Decentralized system design with logical blob grouping and scalable indexing.
- Load Balancing:Implements chunking and random selection for static clusters and rebalancing mechanisms for dynamic clusters.
- System Components:
- Cluster Manager:Organizes data into partitions and tracks their state and location.
- Frontend:Receives and routes requests, serving put, get, and delete operations.
- Datanode:Stores and retrieves data, maintaining additional data structures for performance.
- Partitioning Strategy:
- Ambry groups blobs into virtual units called partitions, decoupling logical and physical placement for transparent data movement.
- Partitions are implemented as append-only logs in large files, with replicas stored on multiple Datanodes.
- Operations include put, get, and delete, with read-write partitions transitioning to read-only upon reaching capacity thresholds.
- Lightweight API and Operation Handling:
- Supports three operations: put, get, and delete.
- Frontend conducts security checks and routes requests to appropriate partitions, which are handled in a multi-master design by any replica.
- Experimental Evaluation:
- Ambry has been in production for over two years, serving millions of users across multiple datacenters.
- Achieves high throughput, low latency, and improved load balancing, with experimental results demonstrating its efficiency and scalability.### Summary The consistency levels in Cassandra control replica involvement and acknowledgment requirements for operations, balancing durability and latency. Ambry uses asynchronous writes and proxy requests to manage latency and consistency. Load balancing is achieved in static and dynamic clusters to mitigate skewed workloads and expansions, improving throughput and latency.

Facts

- Cassandra's consistency levels determine the number of replicas involved and acknowledgments needed for operations.
- Ambry uses asynchronous writes to manage latency and consistency, with synchronous writes only in the local datacenter.
- Proxy requests are used to ensure read-after-write consistency across datacenters, occurring infrequently.
- Load balancing in Ambry is achieved in static clusters by splitting large blobs and routing put operations randomly.
- Dynamic clusters experience load imbalances due to skewed read-write partition distribution after expansions.
- Ambry employs a rebalancing mechanism to achieve load balance with minimal data movement.
- The Cluster Manager maintains hardware and logical layouts, syncing state across datacenters using Zookeeper.
- Frontends handle requests, security checks, and capture operations, relying on the Router Library for core logic.
- The Router Library includes policy-based routing, chunking large blobs, failure detection, and proxy requests.
- Chunking mitigates large blob issues by splitting them into smaller chunks and handling retrieval in parallel.
- Ambry's failure detection mechanism leverages request messages to identify unresponsive Datanodes.
- Proxy requests ensure availability and read-after-write consistency across remote datacenters, occurring minimally.### Summary The experience section details the operation and optimization techniques employed by Datanodes in the Ambry system, including indexing, OS caching, batched writes, file handle management, and zero copy gets. It also discusses the replication algorithm and experimental results regarding throughput, latency, and variance.

Facts

- Datanode Layer Overview:
- Datanodes manage data storage and respond to requests for partition replicas.
- Operations include writing to the end of partition files for Puts and potentially time-consuming Gets.
- Optimization Techniques:
- Indexing blobs: Maintains an index of blob offsets per partition replica to enhance retrieval efficiency.
- Exploiting OS cache: Utilizes OS caching to serve reads from RAM, minimizing latency.
- Batched writes: Aggregates writes for a partition and periodically flushes them to disk to minimize disk seeks.
- Keeping file handles open: Maintains all file handles open at all times for efficient access.
- Zero copy gets: Utilizes a zero-copy mechanism for reading blobs directly from disk to the network buffer.
- Indexing (Section 4.3.1):
- Maintains lightweight, in-memory indexing per replica sorted by blob ID to locate blobs with low latency.
- Uses Bloom filters to reduce lookup latency for on-disk segments.
- Exploiting the OS Cache (Section 4.3.2):
- Utilizes recent data caching by the operating system to serve reads from memory.
- Bounds indexing by splitting it into segments, keeping only the latest segment in memory.
- Replication (Section 5):
- Employs asynchronous replication algorithm to synchronize replicas across partitions.
- Decentralized approach where each replica acts as a master and syncs with others.
- Uses a two-phase replication protocol for finding and replicating missing blobs.
- Experimental Results (Section 6):
- Conducts experiments on throughput and latency using micro-benchmarks with different workloads and blob sizes.
- Analyzes the effect of the number of clients and blob sizes on system performance and latency.
- Discusses the variance in latency and tail distributions in different operation modes.### Summary The text discusses various aspects of performance optimization and operational insights into a distributed storage system, particularly focusing on workload patterns, geo-distributed optimizations, replication mechanisms, load balancing strategies, and simulation-based evaluations.

Facts

- The workload pattern includes scenarios where data is either predominantly cached in RAM or served from disk.
- Exploiting the Linux Cache significantly improves average and maximum latency compared to disk reads.
- Geo-distributed optimizations involve replication lag, bandwidth, and latency analysis among different datacenters.
- Inter-datacenter replication bandwidth is negligible, and intra-datacenter replication latency is relatively high due to added delays.
- Load balancing mechanisms are effective in balancing request rates and disk usage across the system.
- Simulation-based evaluations demonstrate the effectiveness of cluster expansion strategies and load balancing mechanisms over time.
- Data movement during rebalancing is minimal and usually below the minimum required for achieving an ideal state.

Summary

The text describes Ambry, a distributed storage system designed for large immutable media objects called blobs. It addresses issues related to load balancing, horizontal scalability, and low latency across multiple data centers.

Facts

- The improvement in range (max-min) and standard deviation of request rates and disk usage over a 400-week interval is highlighted.
- Results are compared between systems with rebalancing (w/ RB) and without rebalancing (w/o RB).
- Data movement of the rebalancing algorithm at each rebalancing point over a 400-week interval is illustrated.
- Ambry's design is influenced by log-structure file systems (LFS), optimized for write throughput and relying on OS cache for reads.
- Distributed file systems such as NFS, AFS, GFS, HDFS, and Ceph suffer from high metadata overhead and scalability issues, which Ambry aims to resolve.
- Many key-value stores cannot efficiently handle massively large objects and add unnecessary overhead to provide consistency.
- Ambry addresses load imbalance issues that other systems like Haystack and Twitter's blob store do not tackle.
- Ambry focuses on large immutable blobs and is designed for low latency and high throughput in a geographically distributed environment.
- Future work for Ambry includes adaptive replication factor changes, erasure coding for cold data, compression mechanisms, and security enhancements.
- Acknowledgments are given to individuals contributing to Ambry's development and deployment.
- References include various papers and systems related to distributed storage and file systems.
