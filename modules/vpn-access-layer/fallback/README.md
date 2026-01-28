# Fallback System

The Fallback subsystem provides automatic recovery and protocol switching when access paths are blocked, throttled, or degraded.  
It ensures that users can maintain connectivity even under aggressive censorship, regional filtering, or active interference.

Fallback is a core resilience mechanism of the VPN Access Layer.

---

## Purpose

The Fallback subsystem enables:

- Automatic switching between TLS, QUIC, HTTP, and CDN-backed entrypoints  
- Region-specific fallback strategies based on known blocking patterns  
- Real-time detection of failures, throttling, and DPI interference  
- Graceful degradation from high-performance paths to stealth paths  
- Persistent connectivity even when multiple protocols are blocked  
- Integration with camouflage and session-init layers for seamless transitions  

It ensures that the system remains reachable in the most hostile environments.

---

## Directory Structure

This directory includes:

- **region-profiles/**  
  Region-specific fallback strategies and blocking patterns.  
  Includes:
  - `cn.md` — China  
  - `ir.md` — Iran  
  - `ru.md` — Russia  
  - `default.md` — Global fallback profile

- **protocol-fallback.md**  
  Defines fallback chains between TLS, QUIC, HTTP, and CDN transports.

- **failure-detection.md**  
  Mechanisms for detecting blocking, throttling, and handshake interference.

- **fallback.md**  
  High-level specification of fallback behavior and integration.

---

## Key Features

- **Region-aware fallback**  
  Uses country-specific blocking intelligence to choose the best fallback path.

- **Protocol chaining**  
  Automatically transitions between:
  - TLS → QUIC → HTTP → CDN  
  - QUIC → TLS → HTTP → CDN  
  - HTTP → TLS → QUIC → CDN  
  depending on failure type.

- **Real-time failure detection**  
  Monitors:
  - handshake failures  
  - packet loss  
  - latency spikes  
  - DPI resets  
  - throttling patterns  

- **Seamless transitions**  
  Maintains session continuity when possible, minimizing user disruption.

- **Adaptive behavior**  
  Learns from repeated failures and adjusts fallback order dynamically.

- **Modular design**  
  Region profiles, detection logic, and fallback chains are independently extensible.

---

## Integration

The Fallback subsystem connects to:

- **entrypoints/**  
  Selects alternative entrypoints when one is blocked.

- **camouflage/**  
  Adjusts camouflage strategies based on regional DPI behavior.

- **session-init/**  
  Re-initiates negotiation when fallback requires a new transport.

- **emergency-publishing/router/**  
  Ensures routing metadata is preserved across fallback transitions.

Fallback is the system’s “self-healing” mechanism.

---

## Summary

The Fallback subsystem ensures persistent connectivity under censorship.  
By combining region-specific intelligence, protocol chaining, real-time failure detection, and seamless transitions, it provides robust resilience against blocking, throttling, and active interference.

This subsystem is essential for maintaining reliable access in hostile network environments.
