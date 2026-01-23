## Routing Module Overview

The Routing module determines the safe, deterministic delivery path for sanitized and validated content within the Emergency Channel pipeline. Its purpose is to ensure that each submission reaches the correct downstream destination—mirrors, NGOs, archives, or internal queues—without revealing user identity, submission patterns, or network metadata.

Routing decisions must rely exclusively on structural properties, classification tags, and integrity status. No semantic analysis, behavioral inference, or origin‑based routing is permitted.

## Routing Logic

Routing decisions are deterministic and based exclusively on structural properties, classification tags, and integrity status. No semantic analysis, behavioral inference, or origin‑based routing is permitted. The module must never reveal or infer user identity, submission patterns, or network metadata.

### 1. Eligibility Requirements
A submission becomes eligible for routing only after:
- Successful sanitization
- Successful classification
- Successful deduplication
- Successful integrity validation

If any prerequisite fails, the submission is routed to the internal `quarantine` queue.

### 2. Primary Routing Factors
Routing is determined using the following non‑semantic factors:
- `classification_tag` (text | image | video | document)
- `integrity.is_valid`
- `deduplication.is_duplicate`
- `structural_summary` (non‑semantic)
- `destination_policy` (static configuration)

No other fields may influence routing.

### 3. Routing Destinations
The module supports the following deterministic destinations:

- **mirror_outbound**  
  For content intended for public or semi‑public mirrors.

- **ngo_outbound**  
  For content intended for human rights organizations or trusted partners.

- **archive_outbound**  
  For long‑term storage in tamper‑resistant archives.

- **internal_review**  
  For content requiring additional structural validation.

- **quarantine**  
  For invalid, corrupted, or unsafe submissions.

### 4. Routing Rules
Routing follows a strict decision tree:

1. If `integrity.is_valid == false` → `quarantine`
2. If `deduplication.is_duplicate == true` → route to canonical destination
3. If classification tag is unsupported → `quarantine`
4. Apply destination policy:
   - text → ngo_outbound or mirror_outbound
   - image → mirror_outbound or archive_outbound
   - video → archive_outbound
   - document → ngo_outbound or archive_outbound

Destination policy is static and must not depend on content semantics.

### 5. Canonical Routing
If a submission is a duplicate:
- It inherits the canonical submission’s destination
- No new routing decision is computed
- No cross‑submission metadata is exposed

### 6. No Dynamic or Adaptive Routing
- No ML‑based routing
- No behavior‑based routing
- No user‑specific routing
- No time‑based or location‑based routing

Routing must remain stable across all environments and deployments.

## Data Contracts

The Routing module uses strict, versioned data contracts to ensure deterministic routing and safe interoperability with upstream and downstream components. All fields must be metadata‑free and must not reveal user identity, timing, or submission relationships.

### 1. RoutingInput
Represents sanitized, deduplicated, and validated content ready for routing.

Fields:
- `submission_id` — unique identifier for tracking
- `classification_tag` — text | image | video | document
- `integrity`:
  - `is_valid` — boolean
  - `validated_structure` — structural summary (non‑semantic)
- `deduplication`:
  - `is_duplicate` — boolean
  - `canonical_id` — optional, internal reference
- `destination_policy` — static routing configuration
- `size_bytes` — confirmed blob size

Rules:
- No semantic fields allowed
- No user‑derived metadata allowed
- No timestamps or routing history included

### 2. RoutingDecision
Represents the deterministic routing outcome.

Fields:
- `submission_id`
- `destination` — mirror_outbound | ngo_outbound | archive_outbound | internal_review | quarantine
- `reason` — generic, non‑diagnostic category
- `canonical_id` — optional, if inherited from a duplicate
- `notes` — minimal internal notes (non‑semantic)

Rules:
- Destination must be derived solely from structural factors
- No sensitive metadata may be included
- No cross‑submission relationships may be exposed

### 3. RoutingError
Represents a routing failure.

Fields:
- `submission_id`
- `reason` — generic error category (e.g., invalid_integrity, unsupported_classification)
- `notes` — minimal internal notes (never content‑related)

### 4. DestinationPolicy
Static configuration defining routing rules for each classification tag.

Fields:
- `text_policy` — ngo_outbound | mirror_outbound
- `image_policy` — mirror_outbound | archive_outbound
- `video_policy` — archive_outbound
- `document_policy` — ngo_outbound | archive_outbound

Rules:
- Must be static and environment‑independent
- Must not depend on content semantics
- Must not depend on user behavior or submission patterns

## Error Handling

The Routing module is designed to fail safely, deterministically, and without revealing internal logic, user identity, or submission relationships. Any inconsistency, invalid state, or unexpected condition must result in a controlled rejection.

### 1. Invalid Integrity State
Triggered when integrity validation is missing, incomplete, or invalid.

Examples:
- `integrity.is_valid` is false
- Missing structural summary
- Inconsistent size fields

Handling:
- Route to `quarantine`
- Do not attempt recovery or re‑validation

### 2. Unsupported Classification
Triggered when the classification tag is missing or unsupported.

Examples:
- Unknown classification tag
- Corrupted classification metadata

Handling:
- Route to `quarantine`
- Do not attempt reclassification

### 3. Deduplication Conflicts
Triggered when deduplication metadata is inconsistent.

Examples:
- Duplicate marked but no canonical_id
- Conflicting duplicate flags
- Canonical record missing

Handling:
- Route to `internal_review`
- Do not attempt to infer canonical relationships

### 4. Policy Violations
Triggered when destination policy is missing or invalid.

Examples:
- Undefined routing policy for a tag
- Policy conflicts across environments

Handling:
- Route to `internal_review`
- Do not fallback to default policies

### 5. Non‑Deterministic Routing State
Triggered when routing decisions are not reproducible.

Examples:
- Inconsistent routing outcomes across runs
- Non‑stable structural summaries
- Conflicting upstream metadata

Handling:
- Route to `internal_review`
- Do not retry or adapt heuristics

### 6. Resource Exhaustion
Triggered when CPU, memory, or I/O limits are exceeded.

Handling:
- Trigger safe failure path
- Route to `quarantine`
- Securely wipe intermediate data

### 7. Adversarial Inputs
Triggered when content appears intentionally crafted to manipulate routing.

Examples:
- Classification spoofing
- Structural ambiguity attacks
- Repeated adversarial submissions

Handling:
- Route to `quarantine`
- Apply temporary rate limiting
- Log anonymized adversarial indicators only

## Security Notes

The Routing module enforces strict security guarantees to ensure that no sensitive information is introduced, inferred, or leaked during routing. All routing decisions must preserve anonymity, avoid semantic interpretation, and maintain deterministic behavior.

### 1. No Identity or Origin Inference
- Routing must never infer user identity, authorship, device characteristics, or submission origin.
- Decisions must rely solely on structural and integrity‑related fields.
- No stylistic, linguistic, or behavioral analysis is permitted.

### 2. Deterministic Routing
- Routing outcomes must be fully deterministic and reproducible.
- Identical inputs must always produce identical routing decisions.
- Randomized routing, adaptive heuristics, or ML‑based routing are prohibited.

### 3. Zero Metadata Introduction
- Routing must not introduce metadata that could reveal timing, location, or user behavior.
- No timestamps, routing history, or diagnostic metadata may be added.
- Internal notes must be minimal, non‑semantic, and non‑identifying.

### 4. Isolation from Raw Content
- The module never receives raw, unsanitized content.
- All inputs must originate from validated upstream modules.
- Any attempt to bypass sanitization must trigger immediate quarantine routing.

### 5. Minimal Logging
- Logs may contain only anonymized operational metrics.
- No routing decisions, structural summaries, or content‑derived metadata may be logged.
- Logs must be purgeable and non‑persistent in hostile environments.

### 6. Defense Against Correlation Attacks
- Routing decisions must not enable cross‑submission correlation.
- Canonical routing must not expose canonical identifiers externally.
- No fine‑grained routing categories or semantic‑based routing paths are permitted.

### 7. Static Policy Enforcement
- Destination policies must be static and environment‑independent.
- Policies must not depend on content semantics, user behavior, or submission patterns.
- Policy enforcement must not reveal internal thresholds or logic.

These safeguards ensure that the Routing module maintains strong anonymity guarantees and prevents any form of identity inference, behavioral profiling, or cross‑submission linkage.
