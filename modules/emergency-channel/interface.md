
Emergency Publishing Channel — Interface Specification

This document defines all external and internal interfaces of the Emergency Publishing Channel, including submission APIs, mirror synchronization APIs, NGO/media delivery APIs, and internal module interfaces.

1. Submission API

POST /submit  
Submit encrypted content for processing.

Headers  
- Authorization: Bearer <token>  
- X-Anonymous-ID: <randomized-id>

Body  
{
  "payload": "<encrypted-bytes>",
  "type": "text | image | video | document",
  "timestamp": "<client-timestamp>"
}

Response  
{
  "status": "ok",
  "submission_id": "<uuid>"
}

2. Mirror Sync API

POST /mirror/sync  
Used by trusted mirror nodes to synchronize new content.

Body  
{
  "node_id": "<uuid>",
  "last_sync": "<timestamp>"
}

Response  
{
  "items": [
    {
      "submission_id": "<uuid>",
      "payload": "<encrypted-bytes>",
      "type": "text | image | video | document",
      "timestamp": "<server-timestamp>"
    }
  ]
}

3. NGO / Media Delivery API

GET /distribution/feed  
Provides sanitized, verified submissions to trusted partners.

Query Parameters  
- since=<timestamp>  
- limit=<int>

Response  
{
  "items": [
    {
      "submission_id": "<uuid>",
      "content": "<sanitized-content>",
      "type": "text | image | video | document",
      "mirrors": ["node-1", "node-3"],
      "ipfs_cid": "bafy...",
      "timestamp": "<server-timestamp>"
    }
  ]
}

4. Internal Interfaces

Sanitizer  
- strip_metadata(content) — Removes EXIF, GPS, device info, timestamps.  
- normalize(content) — Re-encodes images, transcodes video, cleans text.

Router  
- select_nodes() — Chooses optimal mirror nodes.  
- fallback(node) — Fallback routing when primary node fails.

Storage  
- store_local(content) — Local encrypted storage.  
- store_ipfs(content) — Stores content on IPFS.  
- store_arweave(content) — Optional Arweave storage.

Distributor  
- pushtopartners(content) — Sends sanitized content to NGO/media.  
- updatepublicmirrors(content) — Updates public mirror sites.
