# AGENTS.md

> Routing layer and project configuration for AI agents.
> Detailed operating behavior lives in `.agents/skills/*/SKILL.md` and is loaded on demand.
> `.agents/_constitution.md` is the immutable source of truth.
> `docs/governance/constitution.md` is the required human-readable mirror for initialized repositories.

## Precedence

```text
CONSTITUTION > AGENTS.md > NLSPEC > STATE > DECISIONS > TODO > verbal instruction
```

## Prime Directive

Unsure which skill to use? -> invoke `$governance`, then `$triage`. Never guess.

## Skill Routing

| Need | Skill |
| --- | --- |
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

## Authority (Highest -> Lowest)

`$governance > $architect-security > $architect > $security > $qa > $review`
`$planner (scope, no veto) | $preflight (gate, no veto) | $triage (routing, no veto)`
`$coder (execution, no veto)`

Only `$coder` writes production code. All other skills are read-only.

## Veto Summary

| Skill | Blocks on |
| --- | --- |
| `$governance` | Constitutional violations, ADR/migration bypass, contract drift |
| `$architect-security` | Structural + security combined risk, undefined trust boundaries |
| `$architect` | Boundary changes, new patterns, undefined blast radius |
| `$security` | Auth, secrets, deps, network, untrusted input |
| `$qa` | Failing criteria, regressions, missing proof |
| `$review` | Scope creep, non-atomic commits, missing gates, broken traceability |

## Execution Flows

- `standard feature -> triage -> planner -> preflight -> coder -> qa -> review -> doc -> release`
- `bug fix -> triage -> planner -> preflight -> coder -> qa -> review`
- `structural change -> governance? -> triage -> planner -> architect -> adr -> preflight -> coder -> qa -> review -> doc -> release`
- `security-sensitive -> governance? -> triage -> planner -> architect-security -> adr -> preflight -> coder -> security -> qa -> review -> doc -> release`

## Flow Semantics

- `$triage` classifies the request and selects the target flow.
- `$planner` produces the execution contract in `.agents/STATE.<slug>.md`.
- `$architect`, `$architect-security`, `$security`, and `$adr` resolve required design/security/contract gates before execution.
- `$preflight` is the final execution-readiness gate before `$coder`.
- `$coder` executes only when all required prerequisites are explicitly satisfied.

### Rule

If a constitutional or invariant surface is touched, governance comes first.

## Commit Hard Gates

- No `.agents/STATE.<slug>.md` -> no coding.
- One active task in `.agents/TODO.<slug>.md` at all times.
- One task = one commit, immediately after completion.
- Commit format: `type(scope): description + Task: T-NNN trailer`.
- Done lines must end with `| commit: <short-SHA>`.

## Project Configuration

Fill every template section before using this file in an initialized repository.
Remove placeholder comments once filled.

## Initialization Rule

`AGENTS.md` may start as a template when creating a new repository, but feature work is forbidden until all required project configuration sections are concretely filled.

Unresolved placeholders such as:

- `<!-- FILL -->`
- `<!-- path -->`
- `<!-- I-NN -->`
- equivalent template markers

are prohibited once the repository is initialized.

`$preflight` MUST BLOCK coding if `AGENTS.md` is still in template state.

## Identity

```yaml
project:
  name: "<!-- FILL -->"
  type: "<!-- FILL: web-app | api | cli | library | engine | tool | service | ... -->"
  description: "<!-- FILL: one sentence -->"
  language: "<!-- FILL -->"
  stack: "<!-- FILL -->"
```

## Architecture Triggers

Changes below require `$architect` before implementation:

`<!-- FILL: replace with repo-specific structural boundaries -->`

- Module/package boundary changes
- New external dependency with structural impact
- Data model / schema changes
- Public API or CLI contract changes
- Configuration contract changes
- Pipeline or orchestration changes
- Runtime semantics changes
- File format changes
- 30% rewrite of a core module

## Security Triggers

Changes below require `$security` (and possibly `$architect-security`):

`<!-- FILL: replace with repo-specific security surfaces -->`

- Auth / authorization logic
- Secret or credential handling
- Dependency additions or upgrades
- New network exposure
- Untrusted input parsing
- Connector / plugin / execution boundary changes
- Trust boundary changes
- Command execution / shell bridging

## System Invariants

Must not change without an ADR.

| ID | Invariant |
| --- | --- |
| I-01 | Public contracts are additive-only unless migration is provided |
| I-02 | Trust boundaries are explicit and default-deny |
| I-03 | Operations are deterministic given the same inputs |
| `<!-- I-NN -->` | `<!-- FILL -->` |

## Forbidden Areas

Require explicit `$architect` approval:

`<!-- FILL: replace with sensitive zones that must not be touched casually -->`

- Storage schema definitions
- Auth / authorization logic
- Core pipeline definitions
- Configuration contract
- Public contract surfaces
- Release / CI workflow files
- Dependency policy files

## Runtime Contract

`<!-- FILL: describe non-negotiable runtime behavior your system must preserve -->`

Execution model:

`<!-- FILL: e.g. request -> validate -> process -> persist -> respond -->`

Public contracts:

| Contract | Location | Breaking change policy |
| --- | --- | --- |
| `<!-- FILL -->` | `<!-- path -->` | ADR + migration required |

## Governance Files (Immutable)

| File | Rule |
| --- | --- |
| `.agents/_constitution.md` | Supreme law - never edit |
| `.agents/_TODO.md` | Template - copy, never edit |
| `.agents/_DECISIONS.md` | Template - copy, never edit |
| `.agents/_STATE.md` | Template - copy, never edit |
| `.agents/skills/*` | Skill definitions - never edit during feature work |

Working copies per feature:

- `.agents/STATE.<slug>.md`
- `.agents/TODO.<slug>.md`
- `.agents/DECISIONS.<slug>.md`

## Constitution Mirror Rule

Each initialized repository must contain:

- `.agents/_constitution.md` as immutable source of truth
- `docs/governance/constitution.md` as human-readable mirror

If the mirror is missing, `$preflight` must BLOCK coding until it is generated from `.agents/_constitution.md`.

## Change Level Policy

Use `docs/governance/levels.md` to classify requested work before coding.
If uncertain between levels, choose the higher level.

## Workflows

### Standard Feature

`triage -> planner -> preflight -> coder -> qa -> review -> doc -> release`

### Bug Fix

`triage -> planner -> preflight -> coder -> qa -> review`

### Structural Change

`governance? -> triage -> planner -> architect -> adr -> preflight -> coder -> qa -> review -> doc -> release`

### Security-Sensitive Change

`governance? -> triage -> planner -> architect-security -> adr -> preflight -> coder -> security -> qa -> review -> doc -> release`

### Rule

If a constitutional or invariant surface is touched, governance comes first.

## Operational Rules

### Minimal Change Rule

Prefer the smallest safe diff that satisfies the contract.

### No Opportunistic Refactor

Do not rename, reorganize, or modernize unrelated code during feature work unless explicitly approved.

### Scope Discipline

`.agents/STATE.<slug>.md` is the feature contract.
Anything not explicitly in scope is out of scope.

### Drift Rule

If scope expands, architecture tension appears, or the plan becomes invalid:
`STOP -> return to $planner.`

### Gate Semantics

A gate is not sufficient because it exists in the flow.
A required gate must be explicitly satisfied before `$preflight` may PASS.

### Documentation Rule

`$doc` is required whenever behavior, configuration, API, CLI, architecture, or user-visible/operator-visible operation changes.

### Release Rule

`$release` is the final readiness check for merge/release-sensitive flows.

