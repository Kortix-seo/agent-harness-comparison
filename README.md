# Kortix is open source, and this repo compares agent harnesses from their own sources

Kortix is the open-source CompanyOS, and this repository is a comparison of agent harnesses rebuilt from each project's own repository and documentation. Six rows, four questions per row, one source per cell. There is no ranking and no popularity metric. Neither answers the question a team actually asks: what can this thing do to my repository when nobody is watching.

The argument the table makes is narrow enough to check. The approval rule decides which actions run without a person. It is the part of a harness a team argues about. In most of the projects below it lives outside the repository: a home-directory config, an IDE setting, or a command-line flag. A change that widens it never appears in a diff, so the review that approves the work never sees it.

## What is in here

- `README.md`: the table, the validated manifest, and the links.
- `docs/01-where-the-approval-rule-lives.md`: five harnesses, five places the rule lives, and what each costs a reviewer.
- `docs/02-what-a-session-leaves-behind.md`: where the record of a session lands, and what that decides about auditing it later.
- `docs/03-check-a-manifest-in-one-command.md`: how to run `kortix validate` and how to read its output.
- `docs/04-faq.md`: six questions readers arrive with.
- `docs/sources.md`: one line per source, with the date it was read.
- `kortix.yaml`, `agents/harness-auditor.md`, `skills/harness-audit/SKILL.md`: the project that keeps the table current.

## The comparison

Every rival cell is that project's own words or its own documented default, read on 2026-11-02. Kortix is the first row because the comparison is kept here. It is also the one row where the approval rule is a file in the repository being reviewed.

| | What it is | What it owns | The approval rule | What survives |
|---|---|---|---|---|
| Kortix | The open-source CompanyOS | The platform: agents, skills, memory, connectors, and the harness each session runs | `policy.default_mode` and the `read-only` policy set, in `kortix.yaml` | The project repo; work lands through a change request |
| Codex | "Lightweight coding agent that runs in your terminal" | The loop, an OS-enforced sandbox, and an approval policy | Sandbox mode plus approval policy, in `~/.codex/config.toml` | Runtime state and history on the machine that runs it |
| OpenCode | "The open source coding agent." | The loop, tools, subagents, and a permission config | `permission` rules resolving to allow, ask or deny, in `opencode.json` | Session history in its own local data directory |
| Cline | "Autonomous coding agent as an SDK, IDE extension, or CLI assistant." | The loop, tools, and per-category Auto Approve | Auto Approve, per tool call, in the extension's settings | Checkpoints in a shadow Git repository |
| Aider | "aider is AI pair programming in your terminal" | The loop, file edits, and a commit per change | `--yes-always` on the command line, per invocation | A commit in your own repository's history |
| OpenHands | "The self-hosted developer control center for coding agents and automations." | The loop, tools, and a confirmation policy with a security analyzer | A confirmation policy set on the conversation, or in settings | Conversation history under `~/.openhands/conversations` |

Two cells deserve a note before the table is read as a verdict. OpenCode is the closest of the five to a rule you can review, because its permission config is a project file; the gap is the `--auto` flag, which approves anything not explicitly denied for the length of a run. Codex splits the question in two: a sandbox that limits what the agent can touch, and an approval policy that decides when it must ask. Both are configured in a TOML file at user level.

## The manifest, validated

`kortix.yaml` in this repository is the project that keeps the table honest. One agent, one skill, one connector, no secrets. It was checked with the installed CLI on 2026-11-02:

```
$ kortix validate
  ✓  kortix.yaml is valid

What each agent enables:
  harness-auditor  skills=[harness-audit]  subagents=[none (default-deny)]  commands=[none (default-deny)]  extensions=[none (default-deny)]  connectors=[sources]  secrets=[none (default-deny)]
```

CLI: `Kortix CLI v0.14.15-staging.180365ec+180365ec`, run 2026-11-02.

Each column after the agent's name is a grant. `connectors=[sources]` is one: the agent may call the `sources` connector, which reads the public GitHub API. The four `[none (default-deny)]` columns are `subagents`, `commands`, `extensions` and `secrets`, and an omitted grant grants nothing, so the line describes the file rather than a default the runtime supplies. The agent's tool list is on its `tools:` line, and the command does not print it: `read`, `grep`, `write` and `show`, with no `bash`, because it reads a source through the connector.

## Where the approval rule sits in this project

Approval gates you set. Off until you set them. In this project `policy.default_mode: risk` decides a call that no rule matches: a read runs, and a write or a delete waits for a person. The `sources` connector carries a `read-only` policy set, and the agent is granted it: `get_*` always runs, and `*` blocks, so a read of a public source needs no approval and every other call on that connector is refused. One project rule, `sources.*delete*` → `block`, runs before any policy set, so no agent can loosen it. Widening the rule is a diff in the same file as the agent that runs under it, so it arrives in the same change request as the work. Session work reaches main through a change request, and merge is default-deny for agents.

## How the table stays current

A trigger runs on the first of each month at 09:00 UTC. It re-reads every URL in `docs/sources.md` and rewrites the rows from what each project now says. Then it runs `kortix validate` and opens a change request with the diff. It does not merge. When a source no longer states a fact, the cell is deleted rather than left stale, because an empty cell is honest and a stale one is not.

## The same comparison as a page

The same comparison is published on [Agent Harness Guide](https://agentharnessguide.com) as [agent-harness-comparison](https://agentharnessguide.com/agent-harness-comparison), under the same rule about sources. If you want the term defined before the table, [what is an agent harness](https://agentharnessguide.com/what-is-an-agent-harness) does that part by part. If you arrived looking for a ranking, [best agent harness](https://agentharnessguide.com/best-agent-harness) explains why the answer turns on three facts about your work. And if the question is which of these projects you can read and fork, [open source agent harness](https://agentharnessguide.com/open-source-agent-harness) starts there.

## Run it yourself

Clone the repository and run `kortix validate` at the root, and you get the same output. [Kortix](https://kortix.com) is the product this project runs on, and the code is at [Kortix on GitHub](https://github.com/kortix-ai/CompanyOS).
