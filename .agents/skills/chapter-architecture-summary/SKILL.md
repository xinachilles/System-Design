---
name: chapter-architecture-summary
description: >-
  Generates comprehensive, publication-grade system design architecture summaries for book chapters
  or system design specifications, patterned after Figure-22-Final-Design-Architecture.md.
  Extracts end-to-end data flows, subsystem variants (e.g. pull vs push, partitioned queues),
  step-by-step pipeline descriptions, trade-off matrices, and Obsidian-safe Mermaid diagrams.
---

# Chapter Architecture Summary Skill

This skill guides the creation of end-to-end architecture documentation for system design chapters (such as chapters from *System Design Interview – An Insider's Guide*). It produces clean, highly structured, and visually rich Markdown documents matching the structure and depth of `Figure-22-Final-Design-Architecture.md`.

A first-class requirement of this skill is to **exhaustively identify and highlight all architectural trade-offs** discussed across the source material, providing structured comparison matrices with clear decision criteria.

---

## 1. Document Structure Standard

Every generated chapter summary file must adhere to this standardized layout:

```text
# <System Title>: Figure <N> Final Design Architecture

> **Reference Source**: <Book / Source Title>, Chapter <N>: <Chapter Name>
> **Source Material**: <Path to PDF or spec> (Page <P>, Figure <N>)

---

## 1. End-to-End System Architecture (Figure <N>)
[Obsidian-safe Mermaid flowchart showing all major components from ingest to egress]

---

## 2. Key Subsystem Deep Dives & Architectural Variants
[Detailed sub-diagrams and breakdowns of core architectural debates in the chapter,
 e.g., Pull vs. Push models, Partitioning strategies, Queue-less alternatives]

### 2.1 <Variant A Architecture>
[Mermaid diagram + numbered operational mechanics]

### 2.2 <Variant B Architecture>
[Mermaid diagram + numbered operational mechanics]

### 2.3 <Topic> Trade-Off Matrix (Comprehensive Comparison)
[Structured Markdown table comparing all dimensions, pros, cons, and winners]

---

## 3. In-Depth Step-by-Step Pipeline Description
### Step 1: <Component A> → <Component B>
### Step 2: <Component B> → <Component C>
...
[For each step: Data models, protocols (HTTP/TCP/UDP), failover, buffering, and scaling]
[Include inline callouts highlighting trade-offs specific to that step]

---

## 4. Comprehensive Architectural Trade-Off Analysis
[Dedicated section synthesizing ALL major trade-offs analyzed in the chapter]

### 4.1 Primary Architectural Trade-Offs (Multi-Dimensional Matrices)
[Tables for every major fork in the road: e.g. Pull vs Push, Lambda vs Kappa, TSDB vs RDBMS]

### 4.2 Build vs. Buy Decisions
[Evaluation of custom development vs. industry off-the-shelf tools]

---

## 5. Summary Table of Key Architectural Decisions
[Structured Markdown table summarizing key architectural choices across all layers]
```

---

## 2. Mandatory Trade-Off Extraction & Highlighting Guidelines

Every system design chapter is fundamentally an exploration of engineering trade-offs. The generated document must **never omit any trade-off mentioned in the source material**.

### A. How to Locate Trade-Offs in Source Materials
Actively scan the chapter text for discussions involving:
1. **Architectural Ingestion Models**: Pull vs. Push, Polling vs. Streaming/WebSockets, Direct Ingestion vs. Buffered Queues.
2. **Processing Paradigms & Aggregation Tiers**: 
   - Client-side (Agent) vs. Ingestion Stream (Flink/Spark) vs. Query-side (On-Demand).
   - Real-time stream processing vs. Batch reconciliation (Lambda vs. Kappa).
3. **Windowing & Delivery Semantics**:
   - Tumbling vs. Hopping vs. Sliding vs. Session windows.
   - At-least-once vs. At-most-once vs. Exactly-once processing.
   - Handling late-arriving events (Watermarks vs. Dropping vs. Side outputs).
4. **Storage Engine Selection**:
   - Time-Series DB (TSDB) vs. Relational (RDBMS) vs. Wide-Column (Cassandra) vs. OLAP (ClickHouse/Druid).
   - In-memory (Gorilla/Redis) vs. Disk-backed (LSM-tree/SSTables).
5. **Deduplication Strategies**:
   - Distributed cache (Redis) vs. Bloom filters vs. Database unique constraints.
6. **Build vs. Buy Trade-Offs**:
   - Custom in-house solutions vs. established open-source/commercial services (e.g., custom alerting vs. AlertManager, custom graphing vs. Grafana).

### B. Standard Format for Trade-Off Matrices
Every trade-off must be synthesized into a multi-dimensional markdown table following this format:

| Dimension / Requirement | Alternative A (e.g., Pull Model) | Alternative B (e.g., Push Model) | Winner & Decision Rule |
| :--- | :--- | :--- | :--- |
| **Operational Simplicity & Debugging** | Low overhead; endpoints can be curled directly on localhost. | Requires ingress listeners, load balancers, and authentication. | **Alternative A wins** |
| **Health Monitoring** | Instant failure detection (unreachable host = failed instance). | Hard to distinguish network dropouts from host crashes. | **Alternative A wins** |
| **Short-Lived / Ephemeral Jobs** | Scrape cycle may miss batch jobs that terminate quickly. | Tasks push metrics immediately before process exit. | **Alternative B wins** |
| **Network & Firewall Traversal** | Requires opening inbound ports across all monitored subnets. | Monitored hosts initiate outbound traffic to load balancers. | **Alternative B wins** |
| **Transport Latency & Overhead** | Heavyweight TCP connection establishment. | Lightweight UDP fire-and-forget datagrams. | **Alternative B wins** |
| **Data Integrity & Security** | Ingestion targets are strictly governed by service discovery. | Anyone with network access can push metrics without auth. | **Alternative A wins** |

### C. Visual Trade-Off Callouts
In addition to comparison tables, use GitHub/Obsidian style alerts to highlight key engineering takeaways:
```markdown
> [!IMPORTANT]
> **Key Trade-Off Rule for Interviews**:
> If workloads involve ephemeral serverless functions or cross-datacenter firewalls, prefer the **Push Model**. For long-running stateful services with strict authenticity needs, prefer the **Pull Model**.
```

---

## 3. Critical Obsidian Compatibility Rules

Obsidian's Markdown and Mermaid renderer has specific quirks. **You MUST strictly enforce these formatting rules to prevent rendering crashes like `Cannot read properties of null (reading 'getAttribute')`**:

### Rule 1: No Bidirectional `<-->` in Mermaid Flowcharts
* **Problem**: Obsidian's Mermaid engine throws a runtime `TypeError: Cannot read properties of null (reading 'getAttribute')` when attempting to resolve SVG marker heads on `<-->`.
* **Fix**: Always use two unidirectional arrows on separate lines:
  ```mermaid
  %% WRONG
  TSDB <--> QS
  
  %% CORRECT
  TSDB --> QS
  QS --> TSDB
  ```

### Rule 2: Separate Node Declarations from Connection Edges
* **Problem**: Attaching classDef styles (`:::styleName`) directly to special node shapes (e.g., cylinder `[("...")]`) on connection lines causes parser bugs.
* **Fix**: Declare all nodes, labels, and classes at the top of the Mermaid block, then draw edges using simple IDs:
  ```mermaid
  flowchart LR
      classDef storeStyle fill:#e8f5e9,stroke:#43a047,stroke-width:2px,color:#1b5e20
      
      %% 1. Node declarations
      DB[("Time Series DB")]:::storeStyle
      QS["Query Service"]
      
      %% 2. Clean connections
      DB --> QS
  ```

### Rule 3: Use Standard Unicode Arrows in Markdown Headings
* **Problem**: Using LaTeX/KaTeX math syntax in headings (e.g., `### Step 1: Client $\rightarrow$ Server`) causes Obsidian's outline and heading slugifier to crash with `getAttribute`.
* **Fix**: Always use Unicode characters:
  * Use `→` instead of `$\rightarrow$`
  * Use `↔` instead of `$\leftrightarrow$`
  * Use `≥` or `≤` instead of `$\ge$` or `$\le$`
  * Use standard `%` instead of `\%`

---

## 4. Workflow for Processing a Chapter

When the user provides a new chapter PDF or requests a chapter summary:

### Phase 1: Locate Figures, Content, and Trade-Offs Silently
1. Check the chapter outline and locate the **Final Design Diagram** (usually near the end of Step 3 or in Step 4 "Wrap Up").
2. Extract text, tables, and comparative discussions using Python `fitz` (PyMuPDF) or cached text files silently (avoiding interactive terminal prompts).
3. **Compile an exhaustive list of all trade-offs**, pros/cons discussions, and comparison tables presented throughout the chapter.

### Phase 2: Construct the Mermaid Models
1. **End-to-End Diagram**: Model the full pipeline cleanly using `flowchart LR` or `flowchart TB`, applying cohesive color palettes (pastel blues for sources, purple for collectors, orange for buffers, green for storage, pink for queries, red for alerts).
2. **Subsystem Diagrams**: Create focused diagrams for the chapter's core deep-dive topics (e.g. discovery rings, load balancers, write-ahead logs, caching layers).

### Phase 3: Detail the Pipeline Steps & Inline Trade-Offs
For each pipeline step, document:
* **Payload / Data Model**: Schema, attributes, and tags.
* **Network & Protocols**: Scrape endpoints, push ports, RPC/HTTP/UDP.
* **Buffering & Backpressure**: How spikes are absorbed (Kafka, local disk buffer, queues).
* **Storage Internals**: Indexing, partitioning, compression, and retention/downsampling.
* **Failure Modes**: How network partitions, broker crashes, or DB outages are handled.
* **Step-Specific Trade-Offs**: Note why alternative implementations for this step were rejected.

### Phase 4: Output & Validation
1. Write the final markdown file directly into the chapter's directory.
2. Update the workspace `AGENTS.md` memory file to link to the newly created summary file.
3. Verify that the Mermaid syntax contains no `<-->` arrows or LaTeX in headings.
