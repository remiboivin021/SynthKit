# .agents

This directory contains the immutable governance assets and skill definitions used to operate work safely on this repository.

It is not a scratch space.

It is not a feature workspace.

It is part of the control plane.

---

## Purpose

The `.agents/` directory exists to separate:

- immutable governance assets
- reusable templates
- skill procedures

from:

- per-feature working artifacts
- implementation changes
- temporary execution state

This keeps the system stable and prevents feature work from silently changing the rules that govern it.

---

## What Lives Here

### Immutable constitutional source

- `.agents/_constitution.md`

This is the supreme governing document for the repository.

It is the immutable source of truth.

It must not be edited during feature work.

---

### Immutable templates

- `.agents/_STATE.md`
- `.agents/_TODO.md`
- `.agents/_DECISIONS.md`

These are templates.

They are copied to produce per-feature working files.

They must not be edited in place during feature execution.

---

### Skill definitions

- `.agents/skills/*/SKILL.md`

These files define the detailed behavior of each skill.

They are part of the governance/control system.

They must not be modified during ordinary feature work.

---

## What Does **Not** Live Here

Do not place the following in `.agents/`:

- feature-specific `.agents/STATE.<slug>.md`
- feature-specific `.agents/TODO.<slug>.md`
- feature-specific `.agents/DECISIONS.<slug>.md`
- temporary task notes
- implementation scratchpads
- code patches
- local debugging logs
- ad hoc planning documents for one branch only

Those belong in the working repository context, not in the immutable control layer.

---

## Working Copies

Per-feature working artifacts are created at the repository root:

- `.agents/STATE.<slug>.md`
- `.agents/TODO.<slug>.md`
- `.agents/DECISIONS.<slug>.md`

These are the files actively used during feature work.

### Meaning

- `.agents/STATE.<slug>.md` = feature scope contract
- `.agents/TODO.<slug>.md` = execution rail
- `.agents/DECISIONS.<slug>.md` = local decision memory

---

## Rule: Templates Are Copied, Never Edited In Place

Correct pattern:

```text
.agents/_STATE.md       -> .agents/STATE.<slug>.md
.agents/_TODO.md        -> .agents/TODO.<slug>.md
.agents/_DECISIONS.md   -> .agents/DECISIONS.<slug>.md
```

Incorrect pattern:

- edit `.agents/_STATE.md` during a feature
- edit `.agents/_TODO.md` during a feature
- edit `.agents/_DECISIONS.md` during a feature

If a template is missing, it should be restored or recreated by repository maintenance - not casually rewritten during active feature work.

---

## Rule: Constitution Has Two Forms

Two constitutional files are expected in an initialized repository:

- `.agents/_constitution.md` -> immutable source of truth
- `docs/governance/constitution.md` -> human-readable mirror

The mirror exists for readability and onboarding.

If there is any conflict, `.agents/_constitution.md` wins.

---

## Why This Separation Matters

Without this separation, feature work can accidentally:

- weaken governance rules
- mutate shared templates
- create drift between current work and repository law
- cause future branches to inherit corrupted control artifacts

This repository treats governance assets as part of the system, not as incidental docs.

---

## What Preflight Checks Here

preflight is expected to verify that:

- `.agents/_constitution.md` exists
- `.agents/_STATE.md` exists
- `.agents/_TODO.md` exists
- `.agents/_DECISIONS.md` exists
- immutable templates remain unmodified
- required skill files exist where expected

If these are missing or altered improperly, coding must be blocked.

---

## Ownership Model

### Repository maintainer owns

- `.agents/_constitution.md`
- `.agents/_STATE.md`
- `.agents/_TODO.md`
- `.agents/_DECISIONS.md`
- `.agents/skills/*`

### Planner owns

- `.agents/STATE.<slug>.md`

### Active feature execution maintains

- `.agents/TODO.<slug>.md`
- `.agents/DECISIONS.<slug>.md`

### Coder owns

- production code only

Only `$coder` writes production code.  
That does not give `$coder` ownership over immutable governance assets.

---

## Reading Order

If you are new to this repository, read in this order:

- `AGENTS.md`
- `docs/governance/constitution.md`
- `docs/governance/levels.md`
- `docs/governance/workflows.md`
- `.agents/README.md`

Then read the specific skill files only when needed.

---

## Non-Negotiable Summary

- `.agents/` is control plane, not workspace
- templates are copied, never edited in place
- constitution source is immutable
- skill files are not feature-local
- working artifacts live outside `.agents/`
- if immutable governance assets are missing or altered, execution must stop

