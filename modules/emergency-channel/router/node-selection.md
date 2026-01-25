# Emergency Channel — Node Selection

## 1. Purpose

Node selection is the core mechanism that determines which processing or storage node will handle a given piece of content.  
The goal is to choose the most reliable, efficient, and available node based on real-time system metrics and routing policies.  
Node selection must be deterministic, fast, and resilient to partial system failures.

---

## 2. Selection Criteria

Node selection is based on a combination of metrics and policy rules.  
Each node is evaluated according to the following criteria:

### 2.1 Availability

- Node must be online and responding to health checks  
- Recent failure history must be below acceptable thresholds  
- Node must not be in a degraded or maintenance state  

### 2.2 Latency

- Measured round-trip latency from the ingress node  
- Historical latency trends over short and long windows  
- Latency penalties applied to unstable or fluctuating nodes  

### 2.3 Load

- Current queue depth  
- CPU and memory utilization  
- Active request count  
- Load penalties for nodes nearing capacity  

### 2.4 Storage or Capability Constraints

- Node must support the required content type  
- Node must have sufficient storage capacity  
- Node must support required processing features (e.g., sanitizer strict mode)

## 3. Node Scoring

Each node is assigned a score based on the selection criteria.  
The scoring system is modular, allowing each metric to contribute independently to the final decision.

### 3.1 Base Score

Every node begins with a neutral base score.  
Additional points or penalties are applied depending on real-time metrics.

### 3.2 Availability Score

- Positive score for passing recent health checks  
- Penalties for intermittent failures  
- Heavy penalties for nodes marked as degraded  
- Zero score for nodes currently unreachable  

### 3.3 Latency Score

- Lower latency results in higher scores  
- Recent latency spikes reduce the score  
- Nodes with unstable latency receive additional penalties  

### 3.4 Load Score

- Lower load increases the score  
- High queue depth or CPU usage reduces the score  
- Nodes nearing capacity receive heavy penalties  

### 3.5 Capability Score

- Nodes supporting required features receive a bonus  
- Nodes lacking required capabilities are excluded entirely  
- Optional features may add small bonuses  

---

## 4. Node Filtering

Before scoring, nodes may be filtered out based on hard constraints:

- Node is offline or unreachable  
- Node is in maintenance mode  
- Node does not support the required content type  
- Node lacks required processing capabilities  
- Node storage capacity is insufficient  

Filtering ensures that only valid candidates proceed to the scoring stage.

## 5. Final Selection

After filtering and scoring, the Router selects the node with the highest final score.  
If multiple nodes share the same score, a deterministic tie‑breaking rule is applied:

1. Prefer nodes in the same region as the ingress node  
2. Prefer nodes with lower historical failure rates  
3. Prefer nodes with lower average latency  
4. Fall back to lexicographical ordering of node IDs  

This ensures that node selection is predictable and reproducible.

---

## 6. Failover Handling

If the selected node becomes unavailable during routing:

- The Router immediately retries with the next highest‑scoring node  
- Failed nodes are temporarily penalized to avoid repeated selection  
- If all nodes in a region fail, routing automatically expands to other regions  
- Failover must complete within strict time limits to avoid blocking the pipeline  

Failover logic ensures that content is always routed to a healthy node, even during partial outages.

---

## 7. Summary

Node selection is a deterministic, metrics‑driven process that evaluates availability, latency, load, and capabilities.  
By combining filtering, scoring, and failover logic, the Router ensures that every piece of content is processed by the most suitable node available.
