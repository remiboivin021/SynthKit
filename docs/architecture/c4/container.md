# C4 Container View

This document shows the main runtime containers that make up the system.

```mermaid
flowchart TB
    C1[Client or Caller]
    C2[Application Runtime]
    C3[Data Store]
    C4[External Integration]
    C1 --> C2
    C2 --> C3
    C2 --> C4
```

Use this view to document the major deployable units, their responsibilities, and the allowed communication paths.

---
Maintainer/Author: <MAINTAINER_AUTHOR>
Version: 0.1.0
Last modified: 2026-03-01
---
