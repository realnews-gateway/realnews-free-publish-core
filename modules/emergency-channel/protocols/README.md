# Protocols Module

The Protocols module defines the transport layer of the Emergency Channel system.  
It provides a unified abstraction over six censorship‑resistant, high‑performance, and low‑fingerprint protocols:

- **XHTTP** — Hardened HTTP‑based encrypted transport with strong camouflage  
- **XTLS** — High‑performance TLS transport with minimal fingerprint surface  
- **Reality** — Real‑site mimicry with genuine TLS handshakes and active probing resistance  
- **Hysteria2** — QUIC‑based, loss‑resistant, high‑throughput transport  
- **VLESS** — Lightweight, extensible framing protocol used under TLS/XTLS/Reality  
- **Trojan** — TLS‑camouflaged protocol indistinguishable from real HTTPS

These protocols form the system’s multi‑path, censorship‑resistant communication layer.  
The system dynamically selects, combines, or falls back between protocols based on network conditions and threat levels.

---

## Module Structure

This directory contains the following files:

- **overview.md**  
  High‑level description of the protocol stack, design goals, and selection logic.

- **selection.md**  
  Dynamic protocol selection, fallback chains, and adaptive behavior.

- **integration.md**  
  How protocols integrate with Router, Distributor, Publisher, and multi‑hop routing.

- **security.md**  
  Security properties, threat model, and protocol‑specific defenses.

- **xhttp.md**  
  Specification for the XHTTP protocol.

- **xtls.md**  
  Specification for the XTLS transport layer.

- **reality.md**  
  Specification for Reality‑style real‑site mimicry.

- **hysteria2.md**  
  Specification for the Hysteria2 QUIC‑based protocol.

- **vless.md**  
  Specification for the VLESS framing protocol.

- **trojan.md**  
  Specification for the Trojan TLS‑camouflaged protocol.

---

## Key Features

The protocol stack provides:

- **Multi‑protocol redundancy**  
  Multiple independent transports ensure resilience.

- **Dynamic fallback**  
  Automatic switching when a protocol is blocked or degraded.

- **Stealth and camouflage**  
  Reality, Trojan, and XHTTP blend into normal HTTPS/HTTP traffic.

- **High performance**  
  XTLS and Hysteria2 provide optimized data paths for high‑bandwidth or lossy networks.

- **Extensibility**  
  VLESS enables future protocol evolution without breaking compatibility.

- **Multi‑hop diversity**  
  Each hop may use a different protocol for maximum censorship resistance.

---

## Summary

The Protocols module is the foundation of the Emergency Channel’s transport layer.  
By combining six complementary protocols—each optimized for different environments—the system achieves:

- Strong censorship resistance  
- High performance under adverse conditions  
- Stealth against DPI and active probing  
- Flexible, future‑proof architecture  

This module ensures reliable communication even in the most hostile network environments.
