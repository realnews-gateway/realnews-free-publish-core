# Hysteria2 Protocol

## Overview

Hysteria2 is a high‑performance, QUIC‑based transport protocol designed for unstable, high‑latency, or packet‑loss‑heavy networks.  
It prioritizes throughput, congestion resistance, and reliability, making it ideal for environments where traditional TCP‑based protocols degrade or fail entirely.

Hysteria2 is widely used in censorship‑resistant systems due to its ability to maintain stable performance even under aggressive throttling or network interference.

---

## Design Goals

Hysteria2 is built around the following principles:

- **High throughput under packet loss**  
  QUIC‑based congestion control optimized for lossy networks.

- **UDP‑based transport**  
  Avoids TCP head‑of‑line blocking and improves responsiveness.

- **Congestion resistance**  
  Maintains stable performance even under deliberate throttling.

- **Low latency**  
  Faster handshake and recovery compared to TCP‑based protocols.

- **Robustness in hostile environments**  
  Designed to survive poor connectivity and unstable links.

---

## Protocol Structure

Hysteria2 consists of three major layers:

### 1. QUIC Transport Layer
- UDP‑based  
- Built‑in encryption  
- Stream multiplexing  
- Fast handshake  
- Loss‑tolerant congestion control  

This layer provides the foundation for performance and reliability.

### 2. Hysteria2 Control Layer
- Session metadata  
- Flow control  
- Optional padding  
- Adaptive congestion algorithms  

This layer optimizes behavior for real‑world network conditions.

### 3. Encrypted Payload Layer
- AES‑GCM or ChaCha20‑Poly1305  
- Integrity protection  
- Optional forward‑secure rekeying  

Payloads remain secure even on untrusted networks.

---

## Performance Characteristics

Hysteria2 excels in:

- **High packet loss environments**  
  QUIC’s loss recovery is significantly faster than TCP.

- **High latency networks**  
  Reduced handshake overhead improves responsiveness.

- **Mobile or unstable connections**  
  Maintains throughput despite jitter and intermittent loss.

- **Throttled or congested networks**  
  Adaptive congestion control resists bandwidth shaping.

Compared to XTLS or Trojan, Hysteria2 is often the fastest protocol under adverse conditions.

---

## Security Properties

Hysteria2 provides:

- End‑to‑end encryption  
- Integrity protection  
- Replay resistance  
- Metadata minimization  
- Encrypted QUIC handshake  

Because QUIC encrypts most handshake fields, Hysteria2 has a smaller fingerprint surface than many UDP‑based protocols.

---

## Camouflage & DPI Resistance

While not as stealth‑focused as Reality or Trojan, Hysteria2 includes:

- Optional packet size randomization  
- Timing jitter  
- Encrypted handshake  
- No protocol‑specific plaintext identifiers  

These features make it difficult—but not impossible—to fingerprint.

---

## Integration

Hysteria2 is used by:

- Router (for high‑speed upstream links)  
- Distributor (for multi‑hop delivery under poor network conditions)  
- Publisher (for performance‑critical channels)  

It is often placed later in fallback chains due to its lower stealth but superior performance.

---

## Summary

Hysteria2 offers:

- High throughput under packet loss and congestion  
- QUIC‑based, UDP‑optimized transport  
- Strong encryption and integrity protection  
- Excellent performance in unstable or throttled networks  
- A powerful complement to stealth‑focused protocols like Reality and Trojan  

Hysteria2 is the system’s fastest and most resilient protocol for challenging network environments.
