# Modularity Principles

This document defines the coupling and dependency rules that keep the system maintainable.

## Principles

- Separate domain behavior from infrastructure concerns.
- Prefer explicit dependencies over hidden shared state.
- Keep modules focused on one reason to change.
- Avoid cyclic dependencies across boundaries.
- Introduce new boundaries only when they simplify maintenance.

## Enforcement

When a change crosses module boundaries, document why the coupling is justified.

---
Maintainer/Author: <MAINTAINER_AUTHOR>
Version: 0.1.0
Last modified: 2026-03-01
---
