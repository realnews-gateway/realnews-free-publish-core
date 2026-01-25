# Emergency Channel — Local Storage

## 1. Purpose

Local Storage provides a fast, lightweight, and reliable storage backend for the Emergency Channel.  
It is optimized for low-latency writes, temporary caching, and short-term persistence.  
Local Storage is not intended for long-term archival; instead, it acts as the first layer in a multi-tier storage architecture.

---

## 2. Responsibilities

Local Storage is responsible for:

### 2.1 Temporary Content Persistence

- Store incoming content before it is replicated to distributed backends  
- Provide fast read/write access for the core pipeline  
- Ensure content is available during short-term network disruptions  

### 2.2 Caching Layer

- Cache frequently accessed content  
- Reduce load on distributed storage systems  
- Improve performance for sanitization and distribution modules  

### 2.3 Failover Buffer

- Act as a fallback when remote storage is unavailable  
- Queue content for retry during backend outages  
- Prevent data loss during transient failures  

### 2.4 Metadata Management

- Maintain lightweight metadata for indexing and retrieval  
- Track content size, type, and creation timestamps  
- Support efficient cleanup and expiration policies

## 3. Architecture

Local Storage is intentionally simple and lightweight.  
It is designed to operate reliably even on constrained or unstable environments.

### 3.1 Directory Structure

Local Storage organizes content using a predictable directory layout:

    /local-storage/
      /content/
      /metadata/
      /cache/
      /tmp/

- `content/` stores raw content files  
- `metadata/` stores lightweight metadata entries  
- `cache/` holds frequently accessed items  
- `tmp/` is used for transient operations and cleanup tasks  

### 3.2 Storage Engine

The storage engine is built on top of the host filesystem:

- Atomic write operations  
- Safe file renaming for commit semantics  
- Background cleanup tasks  
- Optional compression for large content  

The engine avoids complex indexing to maintain reliability under stress.

### 3.3 Write Path

The write path is optimized for speed:

1. Content is written to a temporary file  
2. Metadata is generated and stored  
3. File is atomically moved into the `content/` directory  
4. Cache entries are updated if necessary  

This ensures that partially written files are never exposed to other modules.

### 3.4 Read Path

Reads are served from:

- Cache (if available)  
- Local content directory  
- Fallback to distributed storage if local copy is missing  

The read path prioritizes speed while maintaining consistency.

## 4. Cleanup and Retention

Local Storage is designed for short-term persistence.  
To prevent uncontrolled growth, it implements strict cleanup and retention policies.

### 4.1 Expiration Policies

Content is removed based on:

- Maximum retention time  
- Storage capacity thresholds  
- Successful replication to distributed backends  
- Explicit cleanup triggers from the pipeline  

Expiration ensures that Local Storage remains lightweight and responsive.

### 4.2 Background Cleanup Tasks

Cleanup tasks run periodically:

- Remove expired content  
- Purge stale cache entries  
- Clear temporary files  
- Compact metadata if necessary  

Cleanup tasks are designed to be low-impact and non-blocking.

### 4.3 Emergency Cleanup Mode

If storage usage exceeds critical thresholds:

- Non-essential cache entries are removed immediately  
- Oldest content is purged first  
- New writes may be temporarily throttled  
- Alerts may be sent to operators (if configured)  

Emergency cleanup prevents system-wide failures during extreme load.

---

## 5. Integration with Distributed Storage

Local Storage works in tandem with distributed backends such as IPFS, Arweave, or cloud object storage.

### 5.1 Replication Workflow

- Content is first written to Local Storage  
- A background task replicates it to distributed storage  
- Replication status is tracked in metadata  
- Local copies may be removed after successful replication  

### 5.2 Read Fallback

If distributed storage is slow or unreachable:

- Local Storage serves as the primary read source  
- Cached copies reduce latency  
- The system avoids unnecessary remote fetches  

### 5.3 Failure Handling

If replication fails:

- Content remains in Local Storage  
- Retry attempts follow exponential backoff  
- Failures are reported to monitoring and analytics  

This ensures durability even when distributed backends are unstable.

---

## 6. Summary

Local Storage provides fast, reliable, and temporary persistence for the Emergency Channel.  
It acts as a caching layer, failover buffer, and first-stage storage backend, ensuring low latency and resilience across the entire pipeline.
