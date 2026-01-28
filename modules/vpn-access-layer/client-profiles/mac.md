# macOS Client Profile

This document provides recommended configuration guidelines for macOS clients connecting to the VPN Access Layer.  
macOS has a unique network stack, TLS behavior, and system-level restrictions that influence transport selection, fallback behavior, and camouflage compatibility.

This profile ensures reliable, censorship-resistant access for macOS users.

---

## Recommended Transport Preferences

macOS performs best with a **TLS-first** strategy due to:

- Strong native TLS stack  
- Predictable certificate handling  
- Stable behavior under DPI interference  
- Compatibility with browser-grade TLS fingerprints  

Recommended order:

1. **TLS (Trojan / XTLS / Reality-style TLS)**
2. **QUIC (Hysteria2)**
3. **HTTP (XHTTP)**
4. **CDN-backed entrypoints**

---

## Camouflage Considerations

macOS TLS fingerprints differ from Windows and Linux.  
Recommended camouflage settings:

- Match Safari or Chrome TLS fingerprints  
- Use genuine certificates for real-site mimicry  
- Enable SNI rotation with macOS-compatible domain lists  
- Normalize packet timing to resemble Safari HTTPS traffic  

---

## Fallback Behavior

macOS should use a conservative fallback chain:

- TLS → QUIC → HTTP → CDN  
- Retry intervals should be slightly longer due to macOS connection caching  
- Enable region-aware fallback (CN/IR/RU profiles)  

---

## System-Level Notes

- macOS aggressively caches DNS; enable DNS padding and randomized TTL  
- QUIC performance varies by version; prefer Hysteria2 with conservative congestion control  
- HTTP-based entrypoints should mimic Safari’s HTTP/2 behavior  

---

## Integration

macOS profiles integrate with:

- **entrypoints/**  
  TLS-first entrypoint selection

- **camouflage/**  
  Safari/Chrome fingerprint shaping

- **fallback/**  
  Region-aware fallback chains

- **session-init/**  
  TLS-first negotiation parameters

---

## Summary

The macOS client profile provides a stable, censorship-resistant configuration optimized for Apple’s network stack.  
By prioritizing TLS, aligning camouflage with Safari/Chrome behavior, and using conservative fallback strategies, macOS users can maintain reliable access even in hostile network environments.
