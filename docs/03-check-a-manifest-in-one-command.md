# Check a manifest in one command, in a repo where Kortix is open source

Kortix is open source, and the manifest in this repository is checked by one command that prints what each agent may use. The command is `kortix validate`. It reads `kortix.yaml`, prints one line per issue, and exits non-zero on any error. A clean run prints a tick and a table.

## The run

From the repository root, with the CLI installed, the run on 2026-11-02 with `Kortix CLI v0.14.15-staging.180365ec+180365ec` printed:

```
$ kortix validate
  ✓  kortix.yaml is valid

What each agent enables:
  harness-auditor  skills=[harness-audit]  subagents=[none (default-deny)]  commands=[none (default-deny)]  extensions=[none (default-deny)]  connectors=[sources]  secrets=[none (default-deny)]
```

## What each column means

The first column is the agent's name, from `agents:` in the manifest. This project has one, `harness-auditor`. The rest of the line describes what it may use.

`skills=[harness-audit]` is the skill the agent may load. It comes from the `skills:` map at the top of the file, which points at `skills/harness-audit`. The agent's own `skills:` list names it too.

The four `[none (default-deny)]` columns are the ones worth reading twice. `subagents`, `commands`, `extensions` and `secrets` are each a grant, and an omitted grant grants nothing. The runtime is not blocking these on the agent's behalf. The file grants none of them, so the agent has no subagent to hand work to, no command to run, no extension to load and no secret to read.

`connectors=[sources]` is the one grant on the line. The agent may call the `sources` connector, which reads the public GitHub API, and the connector's `read-only` policy set decides what each call may do.

The agent's tool list is in the file too, on its `tools:` line, and the command does not print it. It is `read`, `grep`, `write` and `show`, with no `bash`, because the agent reads a source through the connector. That is a gap worth knowing about. The table covers the grants that name other files and services, and the tools are a separate key.

## What the command does not print

The approval rule. `policy.default_mode` is a separate key in the same file, and the `read-only` policy set on the `sources` connector is separate again: `get_*` always runs, and `*` blocks. One project rule, `sources.*delete*` → `block`, runs before any policy set. So the command answers "what may this agent reach", and the file answers "when is a person asked". Both live in the repository. The output is reproducible on any machine, and the rule is reviewable in the same diff as the agent.

## When it fails

The skill in this repository states the contract: one line per issue, and a non-zero exit on any error. That makes the check usable in the monthly audit, where the same command runs before the change request is opened. A manifest that fails exits non-zero, so the check stops the change before the request is opened. The failure is a line a person can read, not a stack trace.

## Why this is the argument in one command

The output is a claim a reader can reproduce, and the rule it does not print is a few lines in the same file. If the rule lived in a home directory, the command would still print the grants. The reviewer would still not see the rule. That is the difference this repository is built to show: the check and the rule are in the same place, so one review covers both.
