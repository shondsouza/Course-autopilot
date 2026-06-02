# Course Pilot 

automates VTU course lecture completion with a local web UI, CLI, and queue-backed server.

Course Pilot logs in to VTU, fetches your course lectures, and submits lecture progress in batches with retry handling.

---

## Features

-  Web UI for easy local access
-  CLI mode for direct automation
-  Live progress via Server-Sent Events (SSE)
-  Queueing with configurable concurrency
-  Automatic session refresh for VTU auth failures
-  Optional Redis-backed stats and persistence
-  Runtime admin config support
