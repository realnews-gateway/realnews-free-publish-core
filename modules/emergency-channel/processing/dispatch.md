## Dispatch Module Overview

The Dispatch module is responsible for delivering routed content to its final destination in a safe, deterministic, and metadata‑free manner. It acts as the final stage of the Emergency Channel processing pipeline, ensuring that validated submissions reach mirrors, NGOs, archives, or internal queues without exposing user identity, network metadata, or submission patterns.

Dispatching must rely exclusively on routing decisions produced by the Routing module. No semantic inspection, behavioral inference, or dynamic decision‑making is permitted at this stage.
