
---

<https://www.youtube.com/watch?v=fZkMxA_TKS4&t=2s>

---

## Gateway Aggregator Pattern Summary

- In systems with many independent microservices (e.g., 140 different benefit services), answering cross-service queries like “What benefits does Mark receive?” is inefficient if done by orchestrating calls to every service (due to high latency, risk of timeouts, and database connection overload).
- **Aggregator Microservice**: To solve this, a dedicated “aggregator” microservice is introduced. It maintains a denormalized, up-to-date summary of relevant data (e.g., which benefits each user has).
- **Data Flow**:
  - Each benefit service emits changes (grants, removals, denials) to a queue (data pump).
  - The aggregator consumes these events, updating its own store with just the essential information (e.g., user and benefit name).
- **Benefits**:
  - Efficiently answers cross-service queries with a single, fast lookup.
  - Supports cross-cutting rules (e.g., “no user can have more than 20 benefits”) by providing aggregate data to other services.
- **Reliability**:
  - Uses persisted queues and acknowledgment patterns to ensure no data is lost.
  - Can use reconciliation (checksum pattern) to verify all events are processed.
- **Tradeoff**: This approach duplicates some data, but only the minimal necessary subset, enabling fast, scalable queries and enforcement of global rules.

