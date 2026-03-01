# AGENTS.md

> Routing layer and project configuration for AI agents.
> Detail lives in `.agents/skills/*/SKILL.md` — loaded on demand.
> Constitution (`.agents/_constitution.md`) always takes precedence.

## Precedence
```
CONSTITUTION > AGENTS.md > NLSpec > STATE > DECISIONS > TODO > verbal instruction
```

## Prime Directive
Unsure which skill to use? → invoke `$governance`, then `$triage`. Never guess.

---

## Skill Routing

| Need | Skill |
|------|-------|
| Governance / invariant / constitutional check | `$governance` |
| Request classification and flow selection | `$triage` |
| Scope definition and planning | `$planner` |
| Execution readiness gate (before coding) | `$preflight` |
| Implementation | `$coder` |
| Structural decisions (architecture) | `$architect` |
| Structural + security combined | `$architect-security` |
| Security review | `$security` |
| Tests and regression validation | `$qa` |
| Final merge gate | `$review` |
| Documentation sync | `$doc` |
| Architecture Decision Record | `$adr` |
| Release / merge readiness | `$release` |

## Authority (highest → lowest)
```
$governance > $architect-security > $architect > $security > $qa > $review
$planner (scope, no veto) | $preflight (gate, no veto) | $triage (routing, no veto)
$coder (execution, no veto)
```

Only `$coder` writes production code. All other skills are read-only.

## Veto Summary

| Skill | Blocks on |
|-------|-----------|
| `$governance` | Constitutional violations, ADR/migration bypass, contract drift |
| `$architect-security` | Structural + security combined risk, undefined trust boundaries |
| `$architect` | Boundary changes, new patterns, undefined blast radius |
| `$security` | Auth, secrets, deps, network, untrusted input |
| `$qa` | Failing criteria, regressions, missing coverage |
| `$review` | Scope creep, non-atomic commits, missing gates |

## Execution Flows

```
feature          → triage → planner → preflight → coder → qa → review → doc → release
bug fix          → triage → planner → preflight → coder → qa → review
structural       → triage → planner → architect → preflight → coder → qa → review → adr → release
security feature → triage → planner → architect-security → preflight → coder → security → qa → review → adr → release
```
Governance is invoked first when a constitutional/invariant surface is touched.

## Commit Hard Gates
- No `STATE.<slug>.md` → no coding.
- One active task in `TODO.<slug>.md` at all times.
- One task = one commit, immediately after completion.
- Commit format: `type(scope): description` + `Task: T-NNN` trailer.
- `Done` lines must end with `| commit: <short-SHA>`.

---

# Project Configuration

> Fill every `<!-- FILL -->` section. Remove placeholder comments once filled.

## Identity
```yaml
project:
  name: "<!-- FILL -->"
  type: "<!-- FILL: web-app | api | cli | library | ... -->"
  description: "<!-- FILL: one sentence -->"
  language: "<!-- FILL -->"
  stack: "<!-- FILL -->"
```

## Architecture Triggers
Changes below require `$architect` before implementation:
<!-- FILL: list your structural boundaries -->
- Module/package boundary changes
- New external dependency
- Data model / schema changes
- Public API or CLI contract changes
- Configuration contract changes
- Pipeline or orchestration changes

## Security Triggers
Changes below require `$security` (and possibly `$architect-security`):
<!-- FILL: list your security surfaces -->
- Auth / authorization logic
- Secret or credential handling
- Dependency additions or upgrades
- New network exposure

## System Invariants
> Must not change without an ADR.

| ID | Invariant |
|----|-----------|
| I-01 | Public contracts are additive-only unless migration is provided |
| I-02 | Trust boundaries are explicit and default-deny |
| I-03 | Operations are deterministic given the same inputs |
| <!-- I-NN --> | <!-- FILL --> |

## Forbidden Areas
Require explicit `$architect` approval:
<!-- FILL: zones that must never be touched casually -->
- Storage schema definitions
- Auth / authorization logic
- Core pipeline definitions
- Configuration contract

## Runtime Contract
<!-- FILL: non-negotiable runtime behavior your system must preserve -->

**Execution model:**
```
<!-- FILL: e.g. request → validate → process → persist → respond -->
```

**Public contracts:**

| Contract | Location | Breaking change policy |
|----------|----------|------------------------|
| <!-- FILL --> | <!-- path --> | ADR + migration required |

---

## Governance Files (Immutable)

| File | Rule |
|------|------|
| `.agents/_constitution.md` | Supreme law — never edit |
| `.agents/_TODO.md` | Template — copy, never edit |
| `.agents/_DECISIONS.md` | Template — copy, never edit |
| `.agents/_STATE.md` | Template — copy, never edit |
| `.agents/skills/*` | Skill definitions — never edit during feature work |

Working copies per feature: `STATE.<slug>.md`, `TODO.<slug>.md`, `DECISIONS.<slug>.md`
