# Threat Model

RealNews Gateway is designed for users in high-risk, heavily censored environments.  
This document outlines the primary adversaries, risks, and mitigation strategies considered during system design.

## Adversaries

### **1. State-Level Censors**
Capabilities include:
- Deep Packet Inspection (DPI)
- Active probing
- Traffic fingerprinting
- IP/domain blocking
- TLS fingerprinting
- QUIC/HTTP/3 interference

### **2. Surveillance Actors**
Capabilities include:
- Metadata collection
- Traffic correlation
- Endpoint monitoring
- Social graph analysis

### **3. Malicious Intermediaries**
Capabilities include:
- Man-in-the-middle attacks
- Content manipulation
- Traffic throttling or injection

## Key Risks

- Protocol fingerprinting and blocking  
- Metadata leakage  
- Node enumeration and takedown  
- Content tampering or censorship  
- User deanonymization  
- Airport-style provider disappearance  

## Mitigation Strategies

### **Protocol-Level**
- Use of modern, low-fingerprint protocols: Hysteria2, Reality, VLESS, Trojan, XTLS, XHTTP  
- Automatic fallback and multi-path routing  
- TLS 1.3 camouflage and QUIC obfuscation  
- Traffic randomization and padding  

### **Application-Level**
- Anonymous posting in BBS  
- Minimal metadata retention  
- Optional decentralized storage for persistence  
- Secure uploader with encryption and metadata stripping  

### **Operational-Level**
- Support for NGO/community-hosted nodes  
- No reliance on single commercial providers  
- Open-source transparency and auditability  
- Modular architecture to isolate failures  

RealNews Gateway is not designed to protect against device-level compromise or targeted surveillance of individuals, but it significantly reduces risks associated with network-level censorship and mass surveillance.
