# Emergency Channel — Distribution Partners

## 1. Purpose

The Distribution Partners module defines how the Emergency Channel integrates with external distribution endpoints.  
These endpoints may include mirror nodes, relay servers, CDN-like partners, community-operated nodes, or trusted organizations that help disseminate sanitized content.

This module documents the requirements, trust boundaries, and operational expectations for all distribution partners.

---

## 2. Partner Categories

Distribution partners fall into several categories, each with different capabilities and trust levels.

### 2.1 Mirror Nodes

Mirror nodes provide:

- Redundant hosting of sanitized content  
- High availability under network pressure  
- Geographically distributed access points  
- Basic caching and static file serving  

Mirror nodes are semi‑trusted and must follow strict content integrity rules.

---

### 2.2 Relay Nodes

Relay nodes forward content to downstream users or other nodes.  
They provide:

- Multi‑hop routing support  
- Obfuscation of origin and destination  
- Traffic smoothing under censorship pressure  

Relay nodes do not store content permanently.

---

### 2.3 CDN‑Like Partners

These partners offer:

- High‑bandwidth distribution  
- Regional caching  
- Burst‑load absorption  
- Fast propagation of sanitized content  

They operate under a higher trust model but must still validate signatures.

---

### 2.4 Community‑Operated Nodes

These nodes are run by volunteers or organizations.  
They provide:

- Additional redundancy  
- Localized distribution  
- Resilience against regional outages  

They operate under a low‑trust model and must rely heavily on signature verification.

---

### 2.5 Institutional Partners

These include NGOs, media organizations, or civil society groups.  
They may:

- Host public mirrors  
- Provide long‑term archival storage  
- Amplify distribution through their own channels  

They operate under a high‑trust model but must still follow integrity rules.

---

## 3. Trust Boundaries

Each partner type has a defined trust boundary:

- **High‑trust partners** may host content directly but must verify signatures.  
- **Medium‑trust partners** may cache content but cannot modify it.  
- **Low‑trust partners** must treat all content as untrusted until verified.  

No partner is allowed to alter sanitized content or inject additional metadata.
