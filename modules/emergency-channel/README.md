# Emergency Publishing Channel

The Emergency Publishing Channel is the core subsystem responsible for secure, anonymous, censorship‑resistant content submission. It ensures that users in hostile environments can safely publish public‑interest information without exposing their identity or metadata.

## Key Features

- **Account‑required access with anonymous publishing**  
  Account identity is strictly separated from publishing identity.

- **Automatic encryption**  
  All submissions are encrypted during transmission and storage.

- **Metadata stripping**  
  Images, videos, and documents are sanitized before processing.

- **Multi‑mirror replication**  
  Content is distributed across multiple nodes for resilience.

- **Decentralized persistence**  
  Optional IPFS/Arweave storage ensures long‑term availability.

- **NGO/media distribution**  
  Trusted partners can receive verified submissions.

## Subcomponents

- `sanitizer/` — Metadata removal and format normalization  
- `router/` — Multi‑node routing and fallback logic  
- `storage/` — Local and decentralized storage handlers  
- `distribution/` — NGO/media and public mirror delivery  

## Design Goals

- Protect user identity  
- Resist state‑level censorship  
- Ensure long‑term content survival  
- Provide a simple, beginner‑friendly submission experience
