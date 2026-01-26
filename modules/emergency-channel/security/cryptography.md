# Security Module — Cryptography

## Overview

The cryptographic layer provides confidentiality, integrity, and authenticity guarantees across the entire Emergency Channel system.  
It defines the primitives, key lifecycles, and verification rules used by all modules, ensuring consistent and secure handling of sensitive data.

Cryptography is applied at multiple stages:  
- During ingestion (hashing, integrity checks)  
- During internal transport (encryption, signing)  
- During storage (at‑rest encryption)  
- During publishing (optional signing or encryption)  

The system is designed to remain secure even under partial compromise.

---

## Cryptographic Primitives

The system uses a combination of modern, well‑vetted primitives:

### Symmetric Encryption
- AES‑256‑GCM  
- ChaCha20‑Poly1305 (fallback for low‑power or unstable environments)

### Asymmetric Encryption
- X25519 for key exchange  
- RSA‑3072 (legacy compatibility)

### Digital Signatures
- Ed25519 (primary)  
- ECDSA P‑256 (optional compatibility)

### Hashing & Integrity
- SHA‑256  
- BLAKE3 (optional high‑performance mode)

### Key Derivation
- HKDF  
- PBKDF2 (legacy compatibility)

All primitives are selected for security, performance, and long‑term maintainability.
