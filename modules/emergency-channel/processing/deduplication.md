## Deduplication Module Overview

The Deduplication module is responsible for detecting identical or near‑identical sanitized submissions. Its purpose is to prevent redundant processing, reduce downstream load, and avoid distributing duplicate content to mirrors, NGOs, or archives.

All deduplication operations must be deterministic, metadata‑free, and resistant to correlation attacks. The module must never reveal relationships between submissions or infer user identity.

## Deduplication Strategy

The Deduplication module uses a multi‑layered, deterministic strategy to detect identical or near‑identical sanitized submissions. All hashing and comparison routines are metadata‑free and designed to resist correlation attacks.

### 1. Primary Hash (Content Hash)
A deterministic hash computed directly from the sanitized blob.

Properties:
- Based solely on content bytes
- Fully deterministic
- Resistant to minor structural variations introduced by sanitization
- Non‑reversible and anonymized

Purpose:
- Detect exact duplicates

### 2. Normalized Hash (Structure Hash)
A secondary hash computed after applying deterministic normalization rules.

Normalization includes:
- Whitespace normalization for text
- Pixel matrix normalization for images
- Container padding normalization for videos
- Object ordering normalization for PDF/A documents

Purpose:
- Detect near‑duplicates that differ only in benign, non‑semantic ways

### 3. Similarity Hash (Optional)
A coarse‑grained hash used only when primary and normalized hashes disagree.

Properties:
- Low‑resolution structural fingerprint
- Deterministic and non‑reversible
- Never used for user‑facing decisions

Purpose:
- Internal clustering and load reduction

### 4. Multi‑Stage Comparison
Deduplication proceeds in the following order:
1. Compare primary hashes  
2. If mismatch → compare normalized hashes  
3. If mismatch → optionally compare similarity hashes  
4. If all mismatch → treat as unique submission

### 5. No Cross‑Submission Linking
- Hashes must never reveal relationships between submissions
- No user identifiers, timestamps, or routing metadata may influence hashing
- Hashes must not be logged or exposed to external modules

## Normalization Rules

Normalization ensures that benign, non‑semantic variations do not prevent deduplication. All normalization routines must be deterministic, metadata‑free, and safe against correlation attacks. Normalization must never alter semantic content.

### 1. Text Normalization
Applied before computing the normalized hash.

Rules:
- Convert all line endings to `\n`
- Collapse multiple spaces into a single space
- Remove trailing whitespace
- Normalize Unicode to NFC
- Strip non‑semantic control characters

Guarantees:
- Text meaning is preserved
- Formatting differences do not affect deduplication

### 2. Image Normalization
Applied to sanitized image blobs.

Rules:
- Normalize pixel matrix ordering
- Remove non‑semantic padding
- Normalize color profile to a deterministic baseline
- Strip any remaining non‑critical chunks (e.g., PNG ancillary chunks)

Guarantees:
- Visual content remains identical
- Minor structural differences do not affect deduplication

### 3. Video Normalization
Applied to sanitized video containers.

Rules:
- Normalize container padding
- Normalize track ordering
- Normalize timestamp bases
- Remove non‑semantic atoms/boxes (e.g., MP4 free atoms)

Guarantees:
- Video playback is unchanged
- Container‑level noise does not affect deduplication

### 4. Document Normalization
Applied to sanitized PDF/A documents.

Rules:
- Normalize object ordering
- Normalize cross‑reference tables
- Remove unused objects
- Normalize whitespace in text streams
- Ensure deterministic page ordering

Guarantees:
- Document content and layout remain unchanged
- Structural noise does not affect deduplication

### 5. Deterministic Failure
If normalization cannot be completed safely:
- Abort normalization
- Fall back to primary hash only
- If primary hash is insufficient → reject submission

## Data Contracts

The Deduplication module uses strict, versioned data contracts to ensure deterministic behavior and safe interoperability with upstream and downstream components. All fields must be metadata‑free and must not reveal relationships between submissions.

### 1. DeduplicationInput
Represents sanitized content received from the Processing Pipeline.

Fields:
- `submission_id` — unique identifier for tracking
- `classification_tag` — text | image | video | document
- `sanitized_blob` — metadata‑free content
- `size_bytes` — size after sanitization

### 2. DeduplicationResult
Represents the output of the deduplication process.

Fields:
- `submission_id`
- `is_duplicate` — boolean
- `duplicate_of` — optional reference to canonical submission (never user‑visible)
- `primary_hash`
- `normalized_hash`
- `similarity_hash` — optional
- `notes` — minimal, non‑diagnostic internal notes

Rules:
- Hashes must never be exposed to external modules
- `duplicate_of` must never reveal user relationships

### 3. DeduplicationError
Represents a deduplication failure.

Fields:
- `submission_id`
- `reason` — generic, non‑diagnostic error category
- `notes` — minimal internal notes (never content‑related)

### 4. CanonicalRecord
Represents the canonical version of a deduplicated submission.

Fields:
- `canonical_id`
- `primary_hash`
- `normalized_hash`
- `similarity_hash` — optional
- `created_at` — normalized server timestamp
- `retention_policy` — internal retention rules

Rules:
- Canonical records must not contain content or metadata
- Canonical records must not be linkable to user identities

## Error Handling

The Deduplication module is designed to fail safely and deterministically. Any ambiguity, corruption, or resource exhaustion must result in a controlled rejection without revealing internal logic or relationships between submissions.

### 1. Structural Errors
Triggered when sanitized content is malformed or incomplete.

Examples:
- Invalid UTF‑8 sequences
- Corrupted image headers
- Video container inconsistencies
- PDF/A structural anomalies

Handling:
- Reject with a generic “invalid structure” reason
- Do not attempt deep repair or inference

### 2. Normalization Errors
Triggered when deterministic normalization cannot be safely applied.

Examples:
- Non‑recoverable Unicode anomalies
- Pixel matrix inconsistencies
- Video timestamp base conflicts
- PDF object ordering failures

Handling:
- Abort normalization
- Fall back to primary hash only
- If primary hash is insufficient → reject submission

### 3. Hashing Errors
Triggered when hashing routines fail or produce inconsistent results.

Examples:
- Hash computation failure
- Non‑deterministic output detected
- Blob size mismatch during hashing

Handling:
- Reject with a generic “hashing failure” reason
- Never retry with alternative algorithms

### 4. Ambiguous Deduplication
Triggered when primary, normalized, and similarity hashes disagree in unexpected ways.

Examples:
- Mixed‑signal blobs
- Content with unstable structural properties

Handling:
- Reject with a generic “ambiguous deduplication” reason
- Do not attempt semantic comparison

### 5. Unsupported Formats
Triggered when sanitized content uses a format outside the supported set.

Handling:
- Reject with a generic “unsupported format” reason
- Do not attempt conversion or normalization

### 6. Resource Exhaustion
Triggered when CPU, memory, or I/O limits are exceeded.

Handling:
- Trigger safe failure path
- Do not return partial results
- Securely wipe intermediate data

### 7. Adversarial Inputs
Triggered when content appears intentionally crafted to bypass deduplication.

Examples:
- Hash collision attempts
- Header spoofing
- Repeated adversarial submissions

Handling:
- Reject and apply temporary rate limiting
- Log anonymized adversarial indicators only

## Security Notes

The Deduplication module enforces strict security guarantees to ensure that no sensitive information is introduced, inferred, or leaked during hashing, normalization, or comparison. All operations must preserve anonymity and maintain deterministic behavior.

### 1. No Identity or Relationship Inference
- Deduplication must never reveal relationships between submissions.
- Hashes must not encode timestamps, routing metadata, or user identifiers.
- Canonical records must not be linkable to user identities.

### 2. Deterministic Hashing
- All hashing routines must be deterministic and reproducible.
- Identical sanitized inputs must always produce identical hashes.
- Randomized hashing, adaptive heuristics, or ML‑based similarity detection are prohibited.

### 3. Zero Metadata Introduction
- Deduplication must not introduce metadata that could reveal timing, location, or user behavior.
- Hashes must be based solely on sanitized content bytes.
- No diagnostic or debugging metadata may be added to results.

### 4. Isolation from Raw Content
- The module never receives raw, unsanitized content.
- All inputs must originate from the Sanitizer or Processing Pipeline.
- Any attempt to bypass sanitization must trigger immediate rejection.

### 5. Minimal Logging
- Logs may contain only anonymized operational metrics.
- No hashes, content, or derived metadata may be logged.
- Logs must be purgeable and non‑persistent in hostile environments.

### 6. Defense Against Correlation Attacks
- Hashes must be anonymized and non‑reversible.
- Normalized and similarity hashes must not enable cross‑submission correlation.
- Deduplication results must not expose canonical record identifiers externally.

These safeguards ensure that the Deduplication module maintains strong anonymity guarantees and prevents any form of cross‑submission linkage or identity inference.
