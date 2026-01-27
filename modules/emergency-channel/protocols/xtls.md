# XTLS Protocol

## Overview

XTLS is a high‑performance, low‑fingerprint transport protocol designed to optimize encrypted data transmission while minimizing detectable patterns.  
It improves upon traditional TLS by reducing overhead, eliminating unnecessary encryption for already‑secure segments, and providing a streamlined data path that is harder to fingerprint.

XTLS is widely used in censorship‑resistant systems due to its combination of stealth, speed, and efficiency.

---

## Design Goals

XTLS is built around the following principles:

- **Reduced fingerprint surface**  
  TLS handshakes and data flows mimic legitimate HTTPS traffic.

- **Optimized data path**  
  Certain segments can bypass redundant encryption for higher throughput.

- **High performance under load**  
  Designed for large data transfers and real‑time communication.

- **Compatibility with existing TLS infrastructure**  
  Works with standard TLS 1.3 semantics while extending the data path.

- **Stealth and indistinguishability**  
  Traffic patterns resemble normal encrypted web traffic.

---

## Protocol Structure

XTLS consists of two major layers:

### 1. TLS Front Layer
- Standard TLS 1.3 handshake  
- Valid certificates  
- Realistic cipher suites  
- Browser‑like handshake patterns  
- No unusual extensions  

This layer ensures indistinguishability from normal HTTPS.

### 2. XTLS Data Path Layer
- Optimized encrypted data frames  
- Optional “direct mode” for bypassing redundant encryption  
- Stream‑based multiplexing  
- Integrity protection  
- Optional padding for anti‑fingerprinting  

The data path is where XTLS achieves its performance advantages.

---

## Handshake Behavior

XTLS uses a normal TLS 1.3 handshake with:

- Realistic ClientHello  
- Valid SNI  
- Standard ALPN values  
- No protocol‑specific identifiers  

After the handshake, the XTLS data path is negotiated implicitly through encrypted metadata.

This makes XTLS extremely difficult to detect via DPI.

---

## Performance Characteristics

XTLS provides:

- **Lower latency**  
  Reduced overhead in the encrypted data path.

- **Higher throughput**  
  Especially effective for large file transfers or streaming.

- **Efficient CPU usage**  
  Less redundant encryption work.

- **Stable performance under congestion**  
  Works well in high‑latency or unstable networks.

---

## Security Properties

XTLS ensures:

- End‑to‑end encryption  
- Integrity protection  
- Replay resistance  
- Metadata minimization  
- No protocol‑specific fingerprints in the handshake  

The optional “direct mode” is carefully scoped to avoid weakening security guarantees.

---

## Integration

XTLS is used by:

- Router (for high‑bandwidth internal links)  
- Distributor (for multi‑hop high‑speed delivery)  
- Publisher (for TLS‑based channels requiring high throughput)  

It can be combined with fallback protocols such as XHTTP or Trojan.

---

## Summary

XTLS offers:

- A high‑performance TLS‑based transport  
- Minimal fingerprint surface  
- Optimized encrypted data path  
- Strong security with improved efficiency  
- Excellent performance under real‑world network conditions  

XTLS is one of the system’s fastest and most stealth‑capable protocols.
