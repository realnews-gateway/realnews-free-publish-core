# Emergency Channel — Video Sanitizer

## 1. Purpose

The Video Sanitizer ensures that all incoming video content is safe, valid, and compliant with the Emergency Channel’s publishing requirements.  
It removes harmful metadata, validates container and codec integrity, enforces duration and resolution limits, and prepares videos for downstream processing such as storage and distribution.

The sanitizer must operate deterministically and avoid altering the semantic meaning of the video unless required for safety or policy compliance.

---

## 2. Responsibilities

The Video Sanitizer is responsible for:

### 2.1 Security Filtering

- Detect and block embedded executable payloads  
- Remove malicious or unsupported metadata fields  
- Validate container structure and codec signatures  
- Reject disguised or malformed video files  

### 2.2 Format Normalization

- Convert unsupported containers to MP4 (H.264/AAC)  
- Normalize frame rate and color space  
- Remove unsupported audio tracks or subtitles  
- Ensure deterministic encoding parameters  

### 2.3 Policy Enforcement

- Enforce maximum duration  
- Enforce maximum resolution and bitrate  
- Validate aspect ratio constraints  
- Reject corrupted or partially downloaded videos  

### 2.4 Metadata Sanitization

- Remove EXIF-like metadata (GPS, device info, timestamps)  
- Remove embedded thumbnails or preview frames  
- Remove application-specific metadata blocks  
- Strip subtitle tracks unless explicitly allowed  

Metadata removal protects user privacy and reduces fingerprinting risks.

---

## 3. Architecture

The Video Sanitizer is implemented as a deterministic, multi‑stage pipeline.  
Each stage performs a specific transformation, and the output of one stage becomes the input of the next.

### 3.1 Processing Stages

The sanitizer pipeline consists of:

1. **Container Validation**  
   - Verify container type (MP4, MKV, MOV)  
   - Validate header integrity and atom structure  
   - Reject malformed or ambiguous containers  

2. **Codec Verification**  
   - Validate video codec (H.264 preferred)  
   - Validate audio codec (AAC preferred)  
   - Reject unsupported or proprietary codecs  

3. **Metadata Removal**  
   - Strip GPS, device identifiers, timestamps  
   - Remove embedded thumbnails and preview frames  
   - Remove subtitle tracks unless explicitly allowed  

4. **Security Filtering**  
   - Detect embedded scripts or payloads  
   - Identify steganographic anomalies (heuristic)  
   - Reject videos with suspicious binary patterns  

5. **Policy Enforcement**  
   - Enforce duration limits  
   - Enforce resolution and bitrate constraints  
   - Validate aspect ratio  
   - Reject corrupted or partially downloaded videos  

6. **Format Normalization**  
   - Convert to MP4 (H.264/AAC) if needed  
   - Normalize frame rate (e.g., 24/30 fps)  
   - Normalize color space (e.g., BT.709)  
   - Ensure deterministic encoding parameters  

7. **Output Encoding**  
   - Re‑encode using safe, stable settings  
   - Remove non‑deterministic compression artifacts  
   - Produce a sanitized, reproducible output video  

Each stage is isolated and independently testable.

---

### 3.2 Deterministic Output

The sanitizer must always produce the same output for the same input:

- No variable bitrate randomness  
- No environment‑dependent encoder behavior  
- No time‑dependent metadata  
- No heuristic‑based transformations that vary between runs  

Determinism ensures predictable downstream processing and reproducible results.

---

### 3.3 Error Handling

Errors are categorized into:

- **Recoverable errors**  
  - Minor metadata corruption  
  - Unsupported but convertible codecs  
  - Missing or invalid container fields  
  - These are corrected automatically  

- **Unrecoverable errors**  
  - Malicious payloads  
  - Severe corruption  
  - Unbounded duration or resolution  
  - These result in rejection  

All errors are logged for analytics and debugging.

---

### 3.4 Performance Considerations

The sanitizer is optimized for:

- Streaming‑based decoding and re‑encoding  
- Low memory usage  
- Linear‑time processing  
- High throughput under load  

It must handle large volumes of video without blocking the pipeline.

---

## 4. Security Considerations

The Video Sanitizer is a critical security boundary.  
It must defend the Emergency Channel against malformed, malicious, or intentionally crafted video inputs.

### 4.1 Threat Model

The sanitizer protects against:

- Embedded executable payloads  
- Script injection via metadata  
- Steganographic channels hidden in video or audio streams  
- Oversized or ultra‑high‑resolution videos intended to exhaust resources  
- Corrupted or malformed container structures  
- Codec spoofing or mixed‑codec attacks  
- Encrypted or obfuscated payloads embedded in frames  

All incoming videos are treated as untrusted.

---

### 4.2 Allowed and Disallowed Content

**Allowed:**

- MP4 (H.264/AAC)  
- MOV or MKV that can be safely converted  
- Videos within duration, resolution, and bitrate limits  
- Deterministic re‑encoded outputs  

**Disallowed:**

- Proprietary or unsupported codecs (HEVC, VP9, AV1 unless explicitly enabled)  
- Animated containers with mixed media streams  
- Videos containing embedded subtitles, chapters, or attachments  
- Files with ambiguous or mismatched MIME types  
- Videos containing encrypted or hidden payloads  
- Ultra‑high‑resolution or unbounded bitrate content  

Disallowed content is removed or the entire submission is rejected.

---

### 4.3 Logging and Auditing

The sanitizer logs:

- Rejected videos  
- Format conversions  
- Metadata removal events  
- Policy violations  
- Suspicious binary or timing patterns  
- Steganography detection alerts  

Logs are anonymized and used for analytics, debugging, and threat monitoring.

---

## 5. Summary

The Video Sanitizer ensures that all video content entering the Emergency Channel is:

- Safe  
- Clean  
- Deterministic  
- Policy‑compliant  
- Free of harmful metadata  
- Encoded in a stable, reproducible format  
- Ready for storage and distribution  

It is a foundational component of the system’s security and reliability.
