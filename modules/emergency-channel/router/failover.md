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
