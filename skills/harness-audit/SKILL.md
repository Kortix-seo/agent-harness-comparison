---
name: harness-audit
description: How to rebuild the agent-harness comparison table from each project's own repository and documentation, and how to check a manifest before you push it.
---

# Harness audit

Kortix is open source, and this skill is part of an open-source project built on
Kortix. It exists so the comparison table in this repository can be rebuilt from
primary sources instead of copied.

## The four columns

Each row answers four questions, and each answer comes from one URL:

| Column | The question | Where the answer lives |
|---|---|---|
| What it is | What does the project call itself? | Its own README, first paragraph |
| What it owns | Which parts of the harness does it implement? | Its own docs, the page on architecture or configuration |
| The approval rule | How does a person decide what runs without them? | Its own docs, the page on permissions, approvals or security |
| What survives | Where does the record of a session land? | Its own docs, the page on state, history or persistence |

## Rules

- One source per cell. If a cell needs two sources, it is two cells.
- Quote the mechanism in the project's own terms. "A `permission` config that
  resolves each rule to allow, ask or deny" is a fact. "Granular permissions"
  is a summary that hides the fact.
- Record the fetch date beside every source in `docs/sources.md`. A row older
  than the last audit is a row nobody has checked.
- When a project renames or merges, the old row is wrong, not merely old.
  Delete it and add the new one in the same change.

## Checking a manifest

Before you push a change to `kortix.yaml`, run:

```bash
kortix validate
```

It prints one line per issue and exits non-zero on any error. A clean run
prints `✓ kortix.yaml is valid` and a table of what each agent enables. Paste
that output into the README with the CLI version and the date: it is the one
claim in this repository a reader can reproduce in one command.
