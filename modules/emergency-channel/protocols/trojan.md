# Trojan Protocol

## Overview

Trojan is a TLS‑camouflaged transport protocol designed to make proxy traffic indistinguishable from ordinary HTTPS connections.  
It embeds encrypted payloads inside standard TLS streams using valid certificates, realistic handshake patterns, and no protocol‑specific identifiers.

Trojan is widely deployed in censorship‑resistant systems due to its simplicity, stability, and strong camouflage properties.

---

## Design Goals

Trojan is built around the following principles:

- **Indistinguishability from HTTPS**  
  All traffic appears as normal TLS 1.3 encrypted data.

- **Simplicity and reliability**  
  Minimal protocol complexity and stable performance.

- **Strong camouflage**  
  Uses real certificates and standard cipher suites.

- **No fingerprintable behavior**  
  No unique headers, extensions, or plaintext markers.

- **Compatibility with existing infrastructure**  
  Works behind CDNs, reverse proxies, and standard web servers.

---

## Protocol Structure

Trojan consists of two layers:

### 1. TLS Layer
- Standard TLS 1.3 handshake  
- Valid certificate for a real domain  
- Realistic ALPN values  
- Browser‑like ClientHello  
- No unusual extensions  

This layer ensures Trojan traffic looks identical to normal HTTPS.

### 2. Trojan Payload Layer
- Encrypted payload frames  
- Authentication via password or key  
- Optional padding  
- Stream‑based transport  

The payload layer is simple and avoids unnecessary metadata.

---

## Handshake Behavior

Trojan uses a normal TLS handshake:

- Real SNI  
- Real certificate  
- Standard cipher suites  
- No protocol‑specific identifiers  

After the handshake, the client sends an encrypted authentication token.  
If authentication fails, the server behaves like a normal HTTPS server.

This provides strong resistance to active probing.

---

## Security Properties

Trojan provides:

- End‑to‑end encryption  
- Integrity protection  
- Replay resistance  
- Metadata minimization  
- Strong camouflage via real TLS  

Because Trojan uses real certificates and standard TLS behavior, blocking it often requires blocking legitimate HTTPS traffic.

---

## Performance Characteristics

Trojan offers:

- **Stable throughput**  
  Reliable performance across most networks.

- **Low overhead**  
  Minimal framing and no redundant encryption.

- **Moderate latency**  
  Comparable to standard HTTPS.

- **High compatibility**  
  Works well with CDNs and reverse proxies.

Trojan is not as fast as XTLS or Hysteria2, but more stealth‑capable than most UDP‑based protocols.

---

## Camouflage & DPI Resistance

Trojan is designed to resist:

- TLS fingerprinting  
- SNI filtering  
- Active probing  
- Traffic pattern analysis  

Its camouflage relies on:

- Real certificates  
- Realistic handshake patterns  
- No protocol‑specific plaintext  
- Standard TLS 1.3 behavior  

This makes Trojan one of the most reliable stealth transports.

---

## Integration

Trojan is used by:

- Router (for stealth upstream links)  
- Distributor (for stable multi‑hop delivery)  
- Publisher (for HTTPS‑based channels)  

It is often placed early in fallback chains due to its strong camouflage.

---

## Summary

Trojan offers:

- TLS‑camouflaged transport indistinguishable from real HTTPS  
- Strong resistance to DPI and active probing  
- Stable, reliable performance  
- Minimal overhead and simple design  
- A powerful complement to Reality and XTLS  

Trojan is one of the system’s most dependable and stealth‑capable protocols.
