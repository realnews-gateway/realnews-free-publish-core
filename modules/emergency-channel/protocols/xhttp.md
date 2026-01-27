# XHTTP Protocol

## Overview

XHTTP is a hardened, censorship‑resistant extension of standard HTTP.  
It preserves the outward appearance of normal HTTP traffic while embedding encrypted, multiplexed, and integrity‑protected payloads inside legitimate‑looking request/response structures.

XHTTP is designed for environments where TLS interception, DPI fingerprinting, or protocol whitelisting make traditional proxy protocols detectable or blockable.  
Its goal is to blend into ordinary web traffic while providing a secure, extensible transport layer for the Emergency Channel system.

---

## Design Goals

XHTTP is built around the following principles:

- **Indistinguishability from normal HTTP**  
  All traffic conforms to valid HTTP semantics and header patterns.

- **Encrypted payload tunneling**  
  Application data is encrypted and embedded inside HTTP bodies.

- **Multiplexing support**  
  Multiple logical streams can share a single HTTP session.

- **Low fingerprint surface**  
  Header order, casing, and timing mimic real browser behavior.

- **Fallback compatibility**  
  Works over HTTP/1.1 and HTTP/2, with optional HTTP/3 support.

- **Deployable anywhere**  
  Can run behind CDNs, reverse proxies, or static hosting providers.

---

## Protocol Structure

XHTTP uses a layered structure:

1. **HTTP Envelope**  
   - Valid HTTP request/response  
   - Browser‑like headers  
   - Optional caching hints for camouflage  

2. **XHTTP Control Layer**  
   - Stream identifiers  
   - Session metadata  
   - Optional padding  

3. **Encrypted Payload Layer**  
   - AES‑GCM or ChaCha20‑Poly1305  
   - Integrity tags  
   - Optional forward‑secure rekeying  

The outer layer looks like ordinary web traffic; the inner layers provide security and multiplexing.

---

## Handshake

XHTTP uses a lightweight handshake:

- Client sends a normal‑looking HTTP request  
- Server responds with a valid HTTP response  
- Encrypted session metadata is exchanged inside body fields  
- No unusual headers or TLS extensions are required  

The handshake is intentionally minimal to avoid detection.

---

## Traffic Camouflage

XHTTP includes several camouflage techniques:

- Browser‑like header ordering  
- Realistic User‑Agent strings  
- Randomized content lengths  
- Optional HTML/JSON wrapper bodies  
- Timing jitter to mimic human browsing patterns  

These techniques reduce the risk of DPI fingerprinting.

---

## Security Properties

XHTTP provides:

- Encrypted and authenticated payloads  
- Optional forward secrecy  
- Replay protection  
- Metadata minimization  
- Resistance to passive and active probing  

XHTTP does not rely on TLS; it can operate over plain HTTP if necessary, though TLS is recommended when available.

---

## Integration

XHTTP is used by:

- Router (for upstream communication)  
- Distributor (for multi‑hop delivery)  
- Publisher (for HTTP‑based channels)  

It can operate alongside other protocols and participate in fallback chains.

---

## Summary

XHTTP offers:

- A censorship‑resistant HTTP‑based transport  
- Strong encryption inside normal‑looking HTTP traffic  
- Low fingerprint surface and high deployability  
- Multiplexing and session management  
- Compatibility with CDNs and reverse proxies  

XHTTP is the system’s most flexible and stealth‑capable protocol for hostile network environments.
