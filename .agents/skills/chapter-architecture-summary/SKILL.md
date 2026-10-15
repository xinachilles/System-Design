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

### 2.3 Trade-Off Comparison Matrix
[Markdown table comparing dimensions, pros, cons, and winners]

---

## 3. In-Depth Step-by-Step Pipeline Description
### Step 1: <Component A> → <Component B>
### Step 2: <Component B> → <Component C>
...
[For each step: Data models, protocols (HTTP/TCP/UDP), failover, buffering, and scaling]

---

## 4. Summary of Key Architectural Decisions & Trade-Offs
[Structured Markdown table summarizing key architectural choices across all layers]
```

---

## 2. Critical Obsidian Compatibility Rules

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

## 3. Workflow for Processing a Chapter

When the user provides a new chapter PDF or requests a chapter summary:

### Phase 1: Locate Figures and Content Silently
1. Check the chapter outline and locate the **Final Design Diagram** (usually near the end of Step 3 or in Step 4 "Wrap Up").
2. Extract the text and diagram labels from the source PDF using Python `fitz` (PyMuPDF) or cached text files.
3. Identify the major architectural debate or design variant in the chapter (e.g., Pull vs. Push, Synchronous vs. Asynchronous, Long Polling vs. WebSockets, Sharding keys).

### Phase 2: Construct the Mermaid Models
1. **End-to-End Diagram**: Model the full pipeline cleanly using `flowchart LR` or `flowchart TB`, applying cohesive color palettes (pastel blues for sources, purple for collectors, orange for buffers, green for storage, pink for queries, red for alerts).
2. **Subsystem Diagrams**: Create focused diagrams for the chapter's core deep-dive topics (e.g. discovery rings, load balancers, write-ahead logs, caching layers).

### Phase 3: Detail the Pipeline Steps
For each pipeline step, document:
* **Payload / Data Model**: Schema, attributes, and tags.
* **Network & Protocols**: Scrape endpoints, push ports, RPC/HTTP/UDP.
* **Buffering & Backpressure**: How spikes are absorbed (Kafka, local disk buffer, queues).
* **Storage Internals**: Indexing, partitioning, compression, and retention/downsampling.
* **Failure Modes**: How network partitions, broker crashes, or DB outages are handled.

### Phase 4: Output & Validation
1. Write the final markdown file directly into the chapter's directory.
2. Update the workspace `AGENTS.md` memory file to link to the newly created summary file.
3. Verify that the Mermaid syntax contains no `<-->` arrows or LaTeX in headings.
