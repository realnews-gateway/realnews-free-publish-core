# Emergency Channel — Document Sanitizer

## 1. Purpose

The Document Sanitizer ensures that all incoming document files (PDF, DOCX, PPTX, spreadsheets, archives, etc.) are safe, valid, and compliant with the Emergency Channel’s publishing requirements.  
It removes active content, strips metadata, validates structure, enforces size limits, and converts documents into safe, standardized formats for downstream processing.

The sanitizer must operate deterministically and must never execute embedded scripts or macros.

---

## 2. Responsibilities

The Document Sanitizer is responsible for:

### 2.1 Security Filtering

- Remove or block embedded scripts, macros, and executable payloads  
- Detect malicious or obfuscated content  
- Validate container integrity (ZIP‑based formats like DOCX/PPTX)  
- Reject encrypted or password‑protected documents  

### 2.2 Format Normalization

- Convert documents to safe, standardized formats (e.g., PDF/A or plain text)  
- Normalize fonts and embedded resources  
- Remove unsupported or proprietary extensions  
- Ensure deterministic rendering  

### 2.3 Policy Enforcement

- Enforce maximum file size  
- Enforce page count limits  
- Validate allowed MIME types  
- Reject corrupted or partially downloaded files  

### 2.4 Metadata Sanitization

- Remove author, device, and software identifiers  
- Remove timestamps and revision history  
- Remove embedded thumbnails or previews  
- Strip application‑specific metadata blocks  

Metadata removal protects user privacy and reduces fingerprinting risks.
