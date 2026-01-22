## Processing Pipeline Overview

The Processing Pipeline defines the deterministic sequence of operations applied to sanitized content. Its purpose is to classify, verify, deduplicate, enrich, and package content for safe downstream distribution. All stages operate exclusively on metadata‑free inputs and must never introduce sensitive information.

The pipeline is designed to ensure structural integrity, prevent correlation attacks, and produce standardized artifacts suitable for mirroring, NGO delivery, and archival.

## Pipeline Stages

The Processing Pipeline consists of six deterministic stages. Each stage operates exclusively on sanitized content and is designed to preserve anonymity, ensure structural integrity, and prepare content for safe distribution.

### 1. Intake & Validation
- Receives `SanitizedContent` from the Sanitizer module.
- Validates structural soundness and confirms metadata‑free status.
- Rejects malformed or incomplete submissions.

### 2. Content Classification
- Determines whether the content is text, image, video, or document.
- Applies lightweight, deterministic heuristics.
- Produces a normalized `classification_tag`.

### 3. Deduplication
- Computes anonymized, deterministic hashes.
- Detects identical or near‑identical submissions.
- Prevents redundant processing and distribution.
- Ensures deduplication does not reveal user relationships.

### 4. Integrity Verification
- Confirms that sanitized content contains no residual metadata.
- Validates structural consistency (e.g., PDF/A compliance, valid image dimensions).
- Generates a deterministic `integrity_hash`.

### 5. Content Enrichment
- Extracts non‑sensitive structural metadata:
  - Text length
  - Image dimensions
  - Video duration and frame count
  - Document page count
- Produces safe, derived metadata for indexing and routing.

### 6. Packaging & Output
- Converts processed content into a standardized internal format.
- Attaches minimal routing metadata.
- Produces `ProcessedContent` for mirroring, NGO delivery, and archival.

## Format‑Specific Pipelines

Each content type follows a dedicated processing pipeline tailored to its structural characteristics. All pipelines operate exclusively on sanitized, metadata‑free content and must remain deterministic.

### 1. Text Pipeline
Steps:
- Normalize encoding to UTF‑8.
- Compute deterministic integrity and deduplication hashes.
- Extract structural metadata (e.g., character count, line count).
- Generate classification and routing tags.

Output:
- Normalized UTF‑8 text with safe derived metadata.

### 2. Image Pipeline
Steps:
- Validate image dimensions and color profile.
- Compute anonymized deduplication and integrity hashes.
- Extract structural metadata (e.g., width, height, aspect ratio).
- Prepare standardized image packaging.

Output:
- Verified image blob with safe structural metadata.

### 3. Video Pipeline
Steps:
- Validate container structure, frame count, and duration.
- Compute deterministic integrity and deduplication hashes.
- Extract structural metadata (e.g., duration, resolution, frame rate).
- Normalize routing metadata for downstream modules.

Output:
- Verified video blob with safe, non‑sensitive metadata.

### 4. Document Pipeline
Steps:
- Validate PDF/A compliance and structural consistency.
- Compute deterministic hashes.
- Extract structural metadata (e.g., page count, text length).
- Prepare standardized document packaging.

Output:
- Verified PDF/A blob with safe derived metadata.

## Format‑Specific Pipelines

Each content type follows a dedicated processing pipeline tailored to its structural characteristics. All pipelines operate exclusively on sanitized, metadata‑free content and must remain deterministic.

### 1. Text Pipeline
Steps:
- Normalize encoding to UTF‑8.
- Compute deterministic integrity and deduplication hashes.
- Extract structural metadata (e.g., character count, line count).
- Generate classification and routing tags.

Output:
- Normalized UTF‑8 text with safe derived metadata.

### 2. Image Pipeline
Steps:
- Validate image dimensions and color profile.
- Compute anonymized deduplication and integrity hashes.
- Extract structural metadata (e.g., width, height, aspect ratio).
- Prepare standardized image packaging.

Output:
- Verified image blob with safe structural metadata.

### 3. Video Pipeline
Steps:
- Validate container structure, frame count, and duration.
- Compute deterministic integrity and deduplication hashes.
- Extract structural metadata (e.g., duration, resolution, frame rate).
- Normalize routing metadata for downstream modules.

Output:
- Verified video blob with safe, non‑sensitive metadata.

### 4. Document Pipeline
Steps:
- Validate PDF/A compliance and structural consistency.
- Compute deterministic hashes.
- Extract structural metadata (e.g., page count, text length).
- Prepare standardized document packaging.

Output:
- Verified PDF/A blob with safe derived metadata.

## Resource Controls

The Processing Pipeline enforces strict resource controls to prevent denial‑of‑service attacks, excessive computation, and correlation risks. All limits are deterministic and applied uniformly across all content types.

### 1. CPU Limits
- Each processing task receives a fixed CPU quota.
- Long‑running operations are terminated deterministically.
- Classification, hashing, and enrichment must complete within predefined time windows.

### 2. Memory Limits
- The sandbox enforces strict memory ceilings.
- Oversized content may be downscaled or rejected.
- No caching of full content blobs beyond the active task is permitted.

### 3. Disk I/O Limits
- Temporary files are stored in an isolated, ephemeral filesystem.
- Maximum temporary storage per task is capped.
- All temporary data must be securely wiped after processing.

### 4. Execution Time Limits
- Each pipeline stage has a maximum execution time.
- Fallback paths must also respect global time limits.
- Tasks exceeding limits are terminated and marked as failed.

### 5. Concurrency Controls
- Only a limited number of processing tasks may run concurrently.
- High‑load conditions trigger queueing or rate limiting.
- Tasks may not spawn subprocesses outside the sandbox.

### 6. Safety‑First Termination
- Any resource exhaustion triggers a safe failure path.
- No partial results may be returned.
- The sandbox is destroyed immediately after termination.

## Security Notes

The Processing Pipeline enforces strict security guarantees to ensure that no sensitive information is introduced, inferred, or leaked during processing. All operations must preserve anonymity and maintain deterministic behavior.

### 1. No Identity Inference
- The pipeline must never infer user identity, origin, or device characteristics.
- Classification and enrichment rely solely on sanitized structural features.
- Behavioral, stylistic, or linguistic fingerprinting is strictly prohibited.

### 2. Deterministic Processing
- All transformations must be deterministic and reproducible.
- Identical sanitized inputs must always produce identical processed outputs.
- Randomized algorithms, adaptive heuristics, or ML‑based inference are not allowed.

### 3. Zero Metadata Introduction
- The pipeline must not introduce any metadata that could reveal timing, location, or user behavior.
- Routing metadata must be minimal, non‑sensitive, and standardized.
- All timestamps must be server‑generated and normalized.

### 4. Isolation from Raw Content
- The pipeline never receives raw, unsanitized content.
- All inputs must originate from the Sanitizer module.
- Any attempt to bypass sanitization triggers immediate rejection.

### 5. Minimal Logging
- Logs may contain only anonymized operational metrics.
- No content, hashes, or derived metadata may be logged.
- Logs must be purgeable and non‑persistent in hostile environments.

### 6. Defense Against Correlation Attacks
- Deduplication hashes must be anonymized and non‑reversible.
- Derived metadata must not enable cross‑submission correlation.
- Processing reports must exclude any sensitive or identifying information.

These safeguards ensure that the Processing Pipeline maintains strong anonymity guarantees and prepares content safely for downstream distribution.
