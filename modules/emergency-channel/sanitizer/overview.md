## Sanitizer Overview

The Sanitizer module is responsible for removing all metadata, fingerprints, and embedded identifiers from incoming content. It ensures that no device, location, software, or user trace remains before the content enters the processing pipeline.

Sanitization is mandatory for all submissions and operates in a secure sandbox to prevent leakage or correlation. The module supports text, image, video, and document formats, applying format‑specific routines to guarantee anonymity.

## Sanitization Stages

The Sanitizer module performs four specialized stages to eliminate metadata, fingerprints, and embedded identifiers from all incoming content. Each stage operates inside a secure sandbox to prevent correlation or leakage.

### 1. Format Detection
- Identifies the true file type using magic‑byte inspection.
- Rejects ambiguous or unsupported formats.
- Selects the appropriate sanitization routine based on content type.

### 2. Metadata Removal
- Strips EXIF, IPTC, XMP, PDF metadata, and embedded thumbnails.
- Removes GPS coordinates, device identifiers, and software signatures.
- Normalizes file headers to eliminate hidden markers.

### 3. Re‑Encoding & Fingerprint Neutralization
- Re‑encodes images, videos, and documents to remove latent fingerprints.
- Converts proprietary formats into safe, open formats.
- Applies deterministic encoding to prevent correlation attacks.

### 4. Content Normalization
- Ensures consistent output structure across all sanitized files.
- Compresses or resizes media when necessary for safety.
- Produces a sanitized blob ready for downstream processing.

## Supported Formats

The Sanitizer module supports a controlled set of formats to ensure reliable metadata removal and fingerprint neutralization. Unsupported or ambiguous formats are rejected during validation.

### 1. Text Formats
Supported:
- TXT (UTF‑8 only)
- Markdown (MD)
- Plain JSON

Notes:
- All text is normalized to UTF‑8.
- Hidden Unicode markers and directionality controls are removed.

### 2. Image Formats
Supported:
- JPEG (re‑encoded to baseline JPEG)
- PNG (re‑encoded to sanitized PNG)
- WebP (lossless or lossy)

Notes:
- All EXIF, XMP, IPTC metadata is removed.
- Images may be recompressed to eliminate latent fingerprints.

### 3. Video Formats
Supported:
- MP4 (H.264/AAC)
- WebM (VP9/Opus)

Notes:
- Videos are re‑encoded to remove embedded GPS, device IDs, and encoder signatures.
- Thumbnails and container metadata are stripped.

### 4. Document Formats
Supported:
- PDF (converted to sanitized PDF/A)
- DOCX (converted to PDF/A)
- ODT (converted to PDF/A)

Notes:
- All embedded metadata, object streams, and revision history are removed.
- Proprietary formats are converted to safe, open formats.

### 5. Unsupported Formats
Rejected:
- RAW camera formats (CR2, NEF, ARW)
- Encrypted PDFs
- DRM‑protected documents
- Executables, archives, and scripts

These restrictions ensure that sanitization remains deterministic, safe, and resistant to correlation attacks.

## Data Contracts

The Sanitizer module exchanges data with upstream validation and downstream processing components using strict, versioned data contracts. These contracts ensure deterministic behavior and prevent metadata leakage.

### 1. SanitizerInput
Represents the validated content passed from the Validation module.

Fields:
- `normalized_type` — Detected file type (text/image/video/document).
- `safe_blob` — Raw content requiring sanitization.
- `metadata_flags` — Indicators of metadata presence.
- `submission_id` — Unique identifier for tracking.

### 2. SanitizationReport
Represents the internal report generated during sanitization.

Fields:
- `removed_metadata` — List of metadata categories removed.
- `reencoded` — Boolean indicating whether re‑encoding occurred.
- `format_conversion` — Any format changes applied.
- `risk_score` — Internal heuristic score for anomaly detection.

### 3. SanitizedContent
Represents the final sanitized output ready for processing.

Fields:
- `content_type` — text | image | video | document.
- `sanitized_blob` — Fully cleaned and normalized content.
- `sanitization_report` — Summary of transformations applied.
- `size_bytes` — Final size after sanitization.

### 4. RejectionRecord
Represents content that failed sanitization.

Fields:
- `submission_id` — Identifier of rejected content.
- `reason` — Generic rejection reason (non‑diagnostic).
- `anonymized_hash` — Hash of the unsafe blob.
- `notes` — Minimal internal notes for debugging.

## Error Handling

The Sanitizer module is designed to fail safely and deterministically. Any ambiguity, corruption, or unremovable metadata results in immediate rejection to protect user anonymity and downstream components.

### 1. Format Errors
Triggered when the file type is unsupported or ambiguous.

Examples:
- RAW camera formats
- Encrypted PDFs
- Files with mismatched magic bytes

Handling:
- Reject without attempting sanitization
- Do not reveal detected format to the client

### 2. Metadata Removal Errors
Triggered when metadata cannot be fully removed.

Examples:
- Proprietary EXIF blocks
- Embedded GPS tracks
- PDF object streams that resist stripping

Handling:
- Reject with a generic “unsafe content” response
- Log anonymized metadata indicators

### 3. Re‑Encoding Failures
Triggered when media cannot be safely re‑encoded.

Examples:
- Corrupted video frames
- Unsupported codec profiles
- Invalid container structures

Handling:
- Attempt fallback re‑encoding
- Reject only after all fallback paths fail

### 4. Fingerprint Neutralization Errors
Triggered when latent fingerprints cannot be reliably removed.

Examples:
- Encoder‑specific noise patterns
- Watermarked frames
- Deterministic compression artifacts

Handling:
- Reject to prevent correlation attacks
- Flag for internal monitoring

### 5. Size or Resource Errors
Triggered when content exceeds safe processing limits.

Examples:
- Extremely large video files
- Oversized images requiring excessive memory

Handling:
- Reject with a generic size‑related message
- Do not reveal internal thresholds

### 6. Adversarial Inputs
Triggered when content appears intentionally crafted to bypass sanitization.

Examples:
- Steganographic payloads
- Files designed to crash re‑encoding routines.
- Repeated malformed submissions

Handling:
- Reject and apply temporary rate limiting
- Log anonymized adversarial indicators.

## Security Notes

The Sanitizer module enforces strict security guarantees to ensure that no metadata, fingerprints, or identifiers survive the sanitization process. These safeguards must never be bypassed.

### 1. Zero Metadata Leakage
- All EXIF, XMP, IPTC, PDF metadata must be removed.
- No original headers or container metadata may remain.
- Hidden thumbnails, GPS tracks, and revision histories must be stripped.

### 2. Deterministic Output
- Sanitized content must be produced using deterministic encoding.
- Identical inputs must yield identical sanitized outputs.
- Non‑deterministic encoders or compression routines are prohibited.

### 3. Secure Sandbox Execution
- All sanitization routines must run inside an isolated sandbox.
- No external network access is permitted during sanitization.
- Temporary files must be wiped immediately after use.

### 4. No Partial Sanitization
- If any metadata or fingerprint cannot be removed, the content must be rejected.
- Partial or best‑effort sanitization is not allowed.
- Rejection must not reveal internal sanitization logic.

### 5. Minimal Logging
- Only anonymized operational metrics may be logged.
- No content, metadata details, or hashes may appear in logs.
- Logs must be purgeable and non‑persistent in hostile deployments.

### 6. Defense Against Correlation Attacks
- Re‑encoding must remove encoder signatures and deterministic artifacts.
- Proprietary formats must be converted to safe, open formats.
- Fingerprint neutralization must be applied consistently across all media.

These rules ensure that the Sanitizer module provides strong anonymity guarantees and prevents any form of user traceability.
