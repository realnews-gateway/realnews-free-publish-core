# Ingest Module — Sources

## Overview

The Ingest module supports multiple external sources for receiving raw content.  
Each source type has its own trust boundary, validation rules, and operational requirements.  
This document defines all supported ingestion sources and their characteristics.

---

## Source Categories

The system supports the following categories of ingestion sources:

- **User Submissions**  
  Direct submissions from individuals through web forms, apps, or secure channels.

- **Automated Crawlers**  
  Scheduled or event‑driven crawlers that fetch content from predefined sources.

- **Partner Feeds**  
  Content provided by trusted or semi‑trusted institutional partners.

- **Mirror Nodes**  
  Distributed nodes that replicate or forward content from external ecosystems.

- **Opportunistic Sources**  
  Low‑bandwidth or fallback channels used during network disruption.

Each category is handled differently based on reliability, trust, and expected content format.

---

## 1. User Submissions

User submissions are the most diverse and least predictable source type.

Characteristics:

- Highly unstructured  
- Potentially malicious  
- Requires strict validation  
- Often includes mixed formats (text, images, metadata)  

User submissions must always be treated as untrusted input.

---

## 2. Automated Crawlers

Crawlers fetch content from predefined URLs or APIs.

Characteristics:

- Predictable structure  
- High volume  
- Requires rate limiting  
- Must handle network failures gracefully  

Crawler output is still considered untrusted until validated.

---

## 3. Partner Feeds

Partner feeds come from organizations or institutions with established relationships.

Characteristics:

- More structured  
- Higher reliability  
- May include metadata or signatures  
- Requires partner‑specific validation rules  

Partner feeds operate under a medium trust boundary.

---

## 4. Mirror Nodes

Mirror nodes replicate content from distributed ecosystems.

Characteristics:

- Useful for decentralized or censorship‑resistant environments  
- May include duplicate or outdated content  
- Requires deduplication downstream  
- Trust level varies by node type  

Mirror nodes help increase resilience and coverage.

---

## 5. Opportunistic Sources

Opportunistic sources are used during extreme network disruption.

Examples:

- Low‑bandwidth relays  
- Offline submissions  
- Delay‑tolerant networks  
- Emergency fallback channels  

These sources prioritize availability over structure or reliability.

---

## Trust Boundaries

Each source type has a defined trust boundary:

- **User submissions**: lowest trust  
- **Crawlers**: low trust  
- **Mirror nodes**: variable trust  
- **Partner feeds**: medium trust  
- **Opportunistic sources**: lowest trust, high uncertainty  

All content must pass through validation and normalization before entering the pipeline.

---

## Summary

The Ingest module supports diverse content sources, each with unique characteristics:

- User submissions for direct input  
- Crawlers for automated collection  
- Partner feeds for structured content  
- Mirror nodes for decentralized replication  
- Opportunistic sources for fallback scenarios  

These sources ensure that the Emergency Channel can operate reliably across a wide range of environments.
