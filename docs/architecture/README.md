# Architecture Documentation

This directory describes the system architecture.

It focuses on **structure, boundaries, and invariants**, not implementation details.

---

## Purpose

Architecture documentation exists to:
- align contributors
- guide design decisions
- constrain AI-generated solutions

---

## Structure

- `index.md`  
  Entry point and architecture overview.

- `assumptions.md`  
  Explicit assumptions the architecture relies on.

- `system-boundaries.md`  
  What is inside vs outside the system.

- `interfaces.md`  
  External and internal interfaces.

- `data-flow.md`  
  Key data flows and lifecycle.

- `deployment.md`  
  Deployment model and environment assumptions.

- `security-architecture.md`  
  Security principles at the architectural level.

- `modularity-principles.md`  
  Rules governing modularity and coupling.

- `c4/`  
  C4-style architectural views (context, container, component, code).

---

## Rules

- Architecture changes require explicit documentation updates
- Major changes should be backed by an ADR
- Diagrams must match textual descriptions