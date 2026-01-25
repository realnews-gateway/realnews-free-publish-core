# Emergency Channel — Router Design

## 1. Purpose

The Router is responsible for selecting the optimal processing path for each piece of content entering the Emergency Channel.  
It ensures that messages are delivered reliably, efficiently, and with minimal latency, even under unstable network conditions or partial system failures.  
The Router does not modify content; it only determines where and how the content should be processed and stored.

---

## 2. Responsibilities

The Router performs several key functions:

### 2.1 Path Selection

- Choose the most suitable processing path based on content type, size, and priority  
- Select nodes or regions with the lowest latency and highest availability  
- Adapt routing decisions dynamically based on real-time system metrics  

### 2.2 Load Balancing

- Distribute traffic evenly across available nodes  
- Prevent overload on any single node or region  
- Shift traffic away from congested or degraded nodes  

### 2.3 Fault Tolerance

- Detect node failures or degraded performance  
- Automatically reroute content to healthy nodes  
- Ensure no single point of failure in the routing layer  

### 2.4 Policy Enforcement

- Apply routing rules based on system configuration  
- Enforce priority handling for critical content  
- Support region-based or content-type-based routing policies

## 3. Routing Architecture

The Router is designed as a lightweight, stateless component that makes fast routing decisions based on real-time system conditions.  
It does not store content or maintain long-term state; instead, it relies on metrics and signals from other modules to determine the optimal path.

### 3.1 Stateless Core

- Routing decisions are computed per request  
- No persistent state is stored inside the Router  
- All stateful information (health, load, availability) is fetched from external modules  

### 3.2 Metrics-Driven Decision Making

Routing decisions are influenced by:

- Node health and heartbeat signals  
- Latency measurements across regions  
- Storage backend availability  
- Sanitizer load and queue depth  
- Distributor performance metrics  

### 3.3 Modular Routing Pipeline

The routing pipeline consists of several independent stages:

- Input normalization  
- Policy evaluation  
- Node scoring  
- Path selection  
- Failover evaluation  
- Final routing decision  

Each stage is isolated and testable, allowing the Router to evolve without breaking the entire system.

## 4. Routing Strategies

The Router supports multiple routing strategies to accommodate different system conditions and content types.  
Strategies can be combined or switched dynamically based on real-time metrics.

### 4.1 Latency-Optimized Routing

- Prioritizes nodes with the lowest measured latency  
- Continuously updates latency scores based on recent requests  
- Ideal for time-sensitive content and high-traffic periods  

### 4.2 Availability-Optimized Routing

- Prefers nodes with the highest uptime and stability  
- Avoids nodes with recent failures or degraded performance  
- Suitable for large or critical content that must not fail  

### 4.3 Load-Aware Routing

- Distributes traffic based on current node load  
- Prevents overload by shifting traffic away from congested nodes  
- Uses queue depth and CPU/memory metrics to estimate load  

### 4.4 Policy-Based Routing

- Applies administrator-defined routing rules  
- Supports region-based, content-type-based, or priority-based policies  
- Ensures compliance with system-wide operational constraints  

---

## 5. Extensibility

The Router is designed to be easily extensible:

- New routing strategies can be added without modifying existing ones  
- Node scoring algorithms can be replaced or enhanced  
- Additional metrics can be integrated into the decision pipeline  
- Failover logic can be extended to support multi-region redundancy  

The modular architecture ensures that improvements to routing logic do not disrupt the rest of the Emergency Channel.

---

## 6. Summary

The Router is a stateless, metrics-driven, and highly extensible component that determines the optimal processing path for all content.  
Its design ensures reliability, low latency, and fault tolerance across the entire Emergency Channel pipeline.
