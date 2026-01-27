# VLESS Protocol

## Overview

VLESS is a lightweight, extensible, and modern transport protocol designed to replace legacy proxy protocols with a cleaner, simpler, and more flexible architecture.  
It removes unnecessary features, avoids legacy baggage, and provides a minimal, efficient framing layer suitable for both high‑performance and stealth‑oriented deployments.

VLESS is widely used as a foundational transport in censorship‑resistant systems due to its simplicity, extensibility, and compatibility with multiple encryption and obfuscation layers.

---

## Design Goals

VLESS is built around the following principles:

- **Simplicity**  
  A minimal protocol without legacy fields or redundant features.

- **Extensibility**  
  Clean framing that supports future protocol extensions.

- **Flexibility**  
  Works with TLS, XTLS, Reality, and other transport layers.

- **Low overhead**  
  Efficient framing with minimal metadata.

- **Neutral design**  
  No built‑in encryption or obfuscation; relies on external layers.

---

## Protocol Structure

VLESS consists of two primary layers:

### 1. VLESS Header
- Version field  
- User ID (UUID)  
- Command type  
- Target address  
- Optional metadata fields  

The header is compact and designed for easy parsing.

### 2. Payload Layer
- Raw data frames  
- Optional padding  
- Optional extension fields  
- No built‑in encryption  

Encryption is provided by the outer transport layer (e.g., TLS, XTLS, Reality).

---

## No Built‑In Encryption

VLESS intentionally does **not** include encryption inside the protocol itself.  
Instead, it relies on the surrounding transport layer for:

- Confidentiality  
- Integrity  
- Replay protection  
- Metadata minimization  

This separation keeps VLESS simple and avoids double encryption.

---

## Extensibility

VLESS supports a clean extension mechanism:

- Optional fields  
- Optional padding  
- Optional metadata blocks  
- Future protocol upgrades without breaking compatibility  

This makes VLESS suitable as a long‑term, future‑proof transport layer.

---

## Performance Characteristics

VLESS provides:

- **Low overhead**  
  Minimal framing and no redundant encryption.

- **High throughput**  
  Especially when combined with XTLS or Hysteria2.

- **Stable performance**  
  Works well across diverse network conditions.

- **Flexible deployment**  
  Can be paired with any of the system’s transport protocols.

---

## Security Properties

VLESS inherits its security from the outer transport layer.  
When paired with TLS, XTLS, or Reality, it provides:

- Strong encryption  
- Integrity protection  
- Metadata minimization  
- Replay resistance  

VLESS itself adds no fingerprintable cryptographic behavior.

---

## Integration

VLESS is used by:

- Router (as a general‑purpose transport)  
- Distributor (for multi‑hop delivery)  
- Publisher (for flexible channel support)  

It is often used as the “base protocol” layered under XTLS or Reality.

---

## Summary

VLESS offers:

- A clean, modern, lightweight transport protocol  
- High extensibility and low overhead  
- Compatibility with TLS, XTLS, Reality, and other layers  
- Strong performance when paired with optimized transports  
- A future‑proof foundation for censorship‑resistant systems  

VLESS is the system’s most flexible and extensible protocol.
