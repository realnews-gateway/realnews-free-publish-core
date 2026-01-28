# Client Profiles

The Client Profiles subsystem provides platform‑specific configuration templates for connecting to the VPN Access Layer.  
These profiles define recommended settings, transport preferences, fallback behavior, and camouflage parameters for each supported platform.

This subsystem ensures that clients on different devices can reliably and safely access the system under censorship.

---

## Purpose

Client Profiles enable:

- Platform‑optimized access configurations  
- Predefined protocol preferences (TLS, QUIC, HTTP, CDN)  
- Region‑aware fallback chains  
- Camouflage‑compatible settings for each OS  
- Minimal user configuration for fast onboarding  
- Consistent behavior across mobile, desktop, and embedded devices  

They provide a standardized way to deploy the VPN Access Layer across diverse environments.

---

## Directory Structure

This directory includes:

- **android.md**  
  Recommended configuration for Android clients, including QUIC‑first profiles and mobile‑optimized fallback.

- **ios.md**  
  iOS‑specific constraints, TLS‑first profiles, and system‑level limitations.

- **windows.md**  
  Desktop‑optimized profiles with high‑performance QUIC and TLS fallback.

- **linux.md**  
  CLI‑friendly profiles for servers, desktops, and embedded Linux systems.

- **embedded.md**  
  Lightweight profiles for routers, IoT devices, and constrained hardware.

- **client-profiles.md**  
  High‑level specification of client profile behavior and integration.

---

## Key Features

- **Platform‑optimized defaults**  
  Each OS receives a configuration tuned for its network stack and limitations.

- **Protocol preference tuning**  
  Example:
  - Android → QUIC‑first  
  - iOS → TLS‑first  
  - Windows → QUIC/TLS hybrid  
  - Linux → configurable  

- **Region‑aware behavior**  
  Profiles automatically reference fallback region profiles.

- **Camouflage compatibility**  
  Ensures TLS fingerprints and SNI behavior match platform expectations.

- **Minimal configuration**  
  Users can connect with minimal manual setup.

- **Extensible design**  
  New platforms (e.g., macOS, OpenWrt) can be added easily.

---

## Integration

Client Profiles connect to:

- **entrypoints/**  
  Define which entrypoints each platform should prefer.

- **fallback/**  
  Reference region‑specific fallback chains.

- **camouflage/**  
  Ensure platform‑specific TLS/QUIC fingerprints are applied.

- **session-init/**  
  Provide negotiation parameters for each platform.

Client Profiles are the user‑facing configuration layer of the VPN Access Layer.

---

## Summary

The Client Profiles subsystem provides platform‑specific configuration templates that ensure reliable, censorship‑resistant access across mobile, desktop, and embedded environments.  
By tuning protocol preferences, fallback behavior, and camouflage settings for each OS, it enables consistent and secure connectivity under hostile network conditions.
