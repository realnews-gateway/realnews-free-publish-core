## Pipeline Overview

The Emergency Channel Pipeline defines the end‑to‑end processing sequence for all incoming submissions. It ensures that every piece of content passes through a consistent, secure, and verifiable workflow before reaching storage or distribution layers.

The pipeline is designed to operate under hostile network conditions, guaranteeing anonymity, integrity, and resilience throughout the entire submission lifecycle.

## Pipeline Stages

The Emergency Channel Pipeline consists of six sequential stages. Each stage enforces strict security, consistency, and anonymity guarantees before passing data to the next component.

### 1. Submission Intake
- Receives encrypted payloads from authenticated users.
- Validates submission tokens and rate limits.
- Strips all transport‑layer metadata before processing.

### 2. Pre‑Sanitization Validation
- Confirms file type, size, and structural integrity.
- Rejects malformed or dangerous payloads.
- Normalizes input into a unified internal format.

### 3. Sanitization
- Removes EXIF, IPTC, XMP, PDF metadata, and embedded thumbnails.
- Re‑encodes media to eliminate hidden fingerprints.
- Ensures no device, location, or software identifiers remain.

### 4. Content Processing
- Computes cryptographic hashes for deduplication.
- Classifies content type (text, image, video, mixed).
- Generates routing metadata for downstream modules.

### 5. Routing & Delivery Preparation
- Selects mirror nodes based on trust, latency, and availability.
- Applies fallback logic for unstable networks.
- Prepares multi‑hop anonymized routing envelopes.

### 6. Persistence & Distribution
- Stores content in encrypted local storage or decentralized networks (IPFS/Arweave).
- Notifies NGO/media partners when applicable.
- Publishes to public mirrors and prepares optional offline bundles.

## Data Contracts

Each stage of the pipeline exchanges data through strict, versioned data contracts. These contracts ensure consistency, verifiability, and forward‑compatibility across all submodules.

### 1. SubmissionPayload
Represents the raw encrypted content submitted by the user.

Fields:
- `encrypted_blob` — Encrypted binary payload.
- `submission_token` — Temporary token validating the session.
- `timestamp` — Client‑side submission time.
- `transport_metadata` — Stripped immediately after intake.

### 2. SanitizedContent
Represents content after metadata removal and format normalization.

Fields:
- `content_type` — text | image | video | document | mixed.
- `normalized_blob` — Cleaned and re‑encoded content.
- `sanitization_report` — Metadata removed, transformations applied.
- `size_bytes` — Final size after normalization.

### 3. ProcessedContent
Represents content after classification and deduplication.

Fields:
- `content_hash` — Cryptographic hash for deduplication.
- `content_class` — text | image | video | mixed.
- `routing_metadata` — Node selection hints.
- `processing_report` — Classification and hash details.

### 4. RoutingEnvelope
Represents the routing‑ready package prepared for mirror nodes.

Fields:
- `target_nodes` — Selected mirror nodes.
- `fallback_nodes` — Backup nodes for unstable networks.
- `multi_hop_path` — Optional anonymized routing chain.
- `payload` — ProcessedContent packaged for delivery.

### 5. PersistedRecord
Represents the final stored or distributed content.

Fields:
- `storage_location` — local | ipfs | arweave.
- `mirror_ids` — Nodes that successfully replicated the content.
- `distribution_channels` — NGO/media, public mirrors, offline bundles.
- `record_hash` — Final integrity hash.

## Error Handling & Recovery

The Emergency Channel Pipeline is designed to remain operational even under partial failure, network interference, or corrupted submissions. Each stage includes explicit error‑handling logic and recovery paths.

### 1. Intake Errors
Common causes:
- Invalid or expired submission token
- Corrupted encrypted payload
- Excessive submission rate

Recovery:
- Reject with a generic failure message (no diagnostic details)
- Regenerate a new submission token when appropriate
- Apply exponential backoff for rate‑limited clients

### 2. Sanitization Errors
Common causes:
- Unsupported file format
- Damaged media files
- Failure during re‑encoding

Recovery:
- Attempt fallback sanitization routines
- Convert to a safe intermediate format when possible
- Reject only when all sanitization paths fail

### 3. Processing Errors
Common causes:
- Hash computation failure
- Classification model timeout
- Inconsistent content structure

Recovery:
- Retry with a secondary hashing or classification method
- Fallback to minimal classification (text/image/video)
- Log anonymized error metadata for debugging

### 4. Routing Errors
Common causes:
- No available mirror nodes
- Network instability
- Multi‑hop path construction failure

Recovery:
- Switch to fallback node list
- Reduce routing complexity (single‑hop mode)
- Queue the payload for delayed delivery

### 5. Persistence Errors
Common causes:
- Local storage write failure
- IPFS/Arweave node unavailability
- Integrity mismatch after replication

Recovery:
- Retry with exponential backoff
- Switch to alternative storage backend
- Mark record as “pending replication” and continue pipeline

### 6. Distribution Errors
Common causes:
- NGO/media endpoint unreachable
- Mirror sync timeout
- Offline bundle generation failure

Recovery:
- Retry on next sync cycle
- Defer distribution until connectivity returns
- Provide fallback to local-only persistence

## Performance Considerations

The Emergency Channel Pipeline is optimized for low‑bandwidth, high‑latency, and intermittently connected environments. Each stage is designed to minimize computational overhead while maintaining strong security guarantees.

### 1. Lightweight Sanitization
- Uses streaming‑based metadata removal to avoid loading full files into memory.
- Re‑encodes media only when necessary to preserve performance.
- Applies format normalization selectively based on content type.

### 2. Efficient Hashing
- Employs fast cryptographic hash functions suitable for large media files.
- Uses incremental hashing to reduce memory usage.
- Caches intermediate results to avoid redundant computation.

### 3. Adaptive Routing
- Prioritizes low‑latency mirror nodes when available.
- Falls back to minimal routing paths in unstable networks.
- Uses compact routing metadata to reduce payload size.

### 4. Storage Optimization
- Compresses content before decentralized storage when safe.
- Uses parallel upload streams for IPFS/Arweave when bandwidth allows.
- Deduplicates content early to reduce storage load.

### 5. Asynchronous Distribution
- NGO/media delivery runs asynchronously to avoid blocking the pipeline.
- Mirror synchronization is batched to reduce network overhead.
- Offline bundle generation is deferred during peak load.

These optimizations ensure that the pipeline remains responsive and reliable even under severe network constraints.

## Security Notes

The Emergency Channel Pipeline enforces strict security controls at every stage to protect users operating under hostile conditions. These notes summarize the mandatory safeguards that must never be bypassed.

### 1. No Identity Linkage
- Submission tokens must never encode user identity.
- Account identifiers must not appear in any downstream data contract.
- Logs must exclude IPs, device IDs, and session identifiers.

### 2. Mandatory Encryption
- All payloads must remain encrypted during transit and storage.
- Decryption is permitted only within the sanitization sandbox.
- Routing envelopes must not expose content or metadata.

### 3. Metadata Elimination
- Sanitization must remove all EXIF, XMP, IPTC, PDF, and embedded metadata.
- Re‑encoding must strip hidden fingerprints and steganographic markers.
- No original file headers may survive after sanitization.

### 4. Integrity Verification
- Every stage must validate hashes before passing data forward.
- Any mismatch requires immediate rejection and regeneration.
- Mirror nodes must verify integrity before replication.

### 5. Minimal Logging
- Only anonymized operational metrics may be logged.
- No content, hashes, or routing metadata may be stored in logs.
- Logs must be purgeable and never written to persistent storage in hostile deployments.

### 6. Fail‑Closed Behavior
- If a stage encounters an unrecoverable error, it must fail closed.
- No partially processed content may continue through the pipeline.
- The system must never leak diagnostic details to the client.

These security notes ensure that the pipeline remains safe, anonymous, and censorship‑resistant even under active surveillance or targeted attacks.
