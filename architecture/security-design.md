# Security Design

RealNews Free Publish Core is built with layered security principles:

## Core Principles

- **Anonymity by design**  
  No persistent identifiers; account and publishing identities are separated.

- **Encryption by default**  
  All content is encrypted during submission, transit, and storage.

- **Transport camouflage**  
  Protocols mimic normal web traffic to evade detection.

- **Decentralized persistence**  
  Content is mirrored across multiple nodes and stored via IPFS/Arweave.

- **No logs, no tracking**  
  The system does not retain user activity or metadata.

## Attack Surface Minimization

- Minimal client footprint  
- No browser dependencies  
- No third‑party analytics  
- No centralized databases

## Out‑of‑Scope Threats

- Compromised devices  
- OS‑level surveillance  
- Physical coercion
