# Emergency Channel — Router Failover

## 1. Purpose

Failover ensures that content continues to flow through the Emergency Channel even when individual nodes or entire regions experience failures.  
The failover system must be fast, deterministic, and capable of handling partial outages without interrupting the publishing pipeline.  
Failover is a core reliability mechanism and must operate automatically without human intervention.

---

## 2. Failure Types

The Router recognizes several categories of failures, each requiring different handling strategies.

### 2.1 Node-Level Failures

- Node becomes unreachable  
- Node fails health checks  
- Node enters a degraded state  
- Node storage or processing capacity is exhausted  

### 2.2 Region-Level Failures

- Multiple nodes in the same region fail simultaneously  
- Regional latency spikes beyond acceptable thresholds  
- Regional storage backend becomes unavailable  
- Network partition isolates the region  

### 2.3 Transient Failures

- Short-lived network instability  
- Temporary queue congestion  
- Brief spikes in CPU or memory usage  
- Intermittent heartbeat failures  

Transient failures must be handled gracefully without overreacting.

---

## 3. Detection Mechanisms

Failover is triggered based on real-time signals from multiple modules:

- Health check failures  
- Latency measurements  
- Queue depth thresholds  
- Storage availability reports  
- Monitoring alerts  
- Distributor delivery failures  

The Router aggregates these signals to determine when failover is necessary.

## 4. Failover Process

Failover follows a structured, deterministic sequence to ensure predictable behavior during outages.

### 4.1 Step 1 — Detect Failure

The Router identifies a failure through:

- Health check timeouts  
- Latency exceeding configured thresholds  
- Queue depth surpassing safe limits  
- Storage backend reporting errors  
- Monitoring alerts indicating node degradation  

Only confirmed failures trigger failover; transient anomalies require multiple signals.

### 4.2 Step 2 — Penalize Failed Node

Once a node is marked as failed:

- Its score is reduced significantly  
- It is temporarily excluded from routing decisions  
- A cooldown timer prevents immediate re-selection  
- Failure metadata is recorded for later analysis  

Penalization prevents repeated routing attempts to unstable nodes.

### 4.3 Step 3 — Select Next Candidate

The Router selects the next highest-scoring node from the candidate list.  
If no nodes remain in the current region, the Router expands the search to other regions.

### 4.4 Step 4 — Retry Routing

The Router attempts to route the content to the new node.  
If this node also fails, the process repeats until a healthy node is found or all options are exhausted.

### 4.5 Step 5 — Escalate if Necessary

If all nodes fail:

- The Router escalates to system-level failover  
- Content may be queued for retry  
- A degraded-mode pipeline may be activated  
- Alerts are sent to operators (if configured)  

Failover escalation ensures that content is never silently dropped.

## 5. Coordination with Other Modules

Failover does not operate in isolation. It relies on close coordination with other modules in the Emergency Channel.

- Monitoring provides health, latency, and queue depth signals  
- Storage reports backend availability and capacity issues  
- Distributor reports delivery failures and regional problems  
- Analytics aggregates failure patterns and long-term trends  
- Core pipeline components expose backpressure and overload signals  

This coordination ensures that failover decisions are based on accurate, system-wide information rather than isolated symptoms.

---

## 6. Recovery and Reintegration

Once a failed node or region recovers, it must rejoin the routing pool safely and gradually.

### 6.1 Recovery Detection

A node is considered recovered when:

- It passes multiple consecutive health checks  
- Latency returns to normal ranges  
- Queue depth stabilizes  
- Storage backend reports full availability  
- No new errors are detected within a defined window  

### 6.2 Gradual Reintegration

Recovered nodes are not immediately treated as fully healthy.  
Instead, the Router applies a staged reintegration process:

- Start with a low initial score  
- Gradually increase score as successful requests accumulate  
- Monitor for regression or repeated failures  
- Fully restore score only after sustained stability  

This prevents unstable nodes from re-entering the pool too aggressively.

### 6.3 Region Reintegration

If an entire region recovers:

- Nodes are reintegrated individually  
- Region-level penalties are removed gradually  
- Latency and availability metrics must stabilize  
- Routing policies may temporarily limit traffic to the region  

Region reintegration ensures global stability during large-scale recoveries.

---

## 7. Degraded Mode

If no healthy nodes are available, the Router enters degraded mode:

- Content may be queued for delayed processing  
- Only minimal validation is performed  
- Non-critical analytics events may be dropped  
- The system prioritizes essential operations over optional ones  

Degraded mode ensures the Emergency Channel remains operational under extreme conditions.

---

## 8. Summary

Failover is a structured, deterministic process that ensures routing continuity during node or region failures.  
Through detection, penalization, retry logic, coordination with other modules, and controlled reintegration, the Router maintains system reliability even under severe or unpredictable conditions.
