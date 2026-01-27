# Reality Protocol

## Overview

Reality is a real‑site‑mimicry transport protocol designed to make encrypted traffic indistinguishable from legitimate TLS connections to real, high‑value websites.  
It leverages authentic domain fronting, genuine TLS handshakes, and server‑side validation to create a transport channel that is extremely difficult to detect or block.

Reality is particularly effective in environments where TLS fingerprinting, SNI filtering, or active probing are used to identify and block proxy protocols.

---

## Design Goals

Reality is built around the following principles:

- **Indistinguishability from real HTTPS traffic**  
  All handshake fields match those of genuine browser connections.

- **Real‑site mimicry**  
  The server presents a certificate for a legitimate domain.

- **Active probing resistance**  
  Unauthorized clients receive a normal website response.

- **Stealth over performance**  
  Prioritizes camouflage rather than raw throughput.

- **No protocol‑specific identifiers**  
  Nothing in the handshake or data path reveals proxy usage.

---

## Protocol Structure

Reality consists of three layers:

### 1. TLS Front Layer
- Valid certificate for a real domain  
- Browser‑like ClientHello and ServerHello  
- Standard cipher suites  
- No unusual extensions  
- Realistic ALPN values  

This layer ensures the connection looks identical to normal HTTPS.

### 2. Reality Validation Layer
- Client sends encrypted validation payload  
- Server verifies authenticity  
- Unauthorized clients fall back to serving the real website  
- No observable difference for invalid clients  

This layer provides stealth and probing resistance.

### 3. Encrypted Data Channel
- Encrypted payload frames  
- Integrity protection  
- Optional padding  
- Stream‑based transport  

Once validated, the encrypted channel behaves similarly to other TLS‑based transports.

---

## Handshake Behavior

The Reality handshake is designed to be indistinguishable from a real browser connection:

- Real SNI  
- Real certificate  
- Real ALPN  
- Real cipher suites  
- Real TLS 1.3 handshake patterns  

After the handshake, the client embeds a small encrypted validation token inside the first data frame.  
If the token is invalid, the server behaves like a normal website.

This makes Reality extremely resistant to active probing.

---

## Security Properties

Reality provides:

- End‑to‑end encryption  
- Strong camouflage  
- Active probing resistance  
- Metadata minimization  
- No detectable protocol signatures  

Because it uses real certificates and real domains, blocking Reality often requires blocking the legitimate site itself.

---

## Performance Characteristics

Reality prioritizes stealth over raw speed:

- Moderate throughput  
- Stable under censorship  
- Works well in high‑risk environments  
- Slightly higher latency due to validation steps  

It is not as fast as XTLS or Hysteria2, but far more stealth‑capable.

---

## Integration

Reality is used by:

- Router (for high‑risk upstream links)  
- Distributor (for stealth multi‑hop delivery)  
- Publisher (for TLS‑based channels requiring maximum camouflage)  

It is often placed early in fallback chains due to its stealth properties.

---

## Summary

Reality offers:

- Real‑site mimicry with genuine TLS handshakes  
- Strong resistance to DPI, SNI filtering, and active probing  
- Seamless fallback to real website behavior  
- Encrypted, integrity‑protected data channel  
- One of the most stealth‑capable transports in the system  

Reality is the protocol of choice for the most hostile and heavily monitored network environments.
