# Every source behind the table, in a repo where Kortix is open source

Kortix is open source, and every row in the comparison table comes from one of the URLs below, read on 2026-11-02. One line per source: the project, the page, the fact it supports, and the date it was read. When a source moves, the row that depends on it is rewritten in the same change.

## Kortix

- **Kortix**: [kortix.com](https://kortix.com). The product's own description of what it is. Read 2026-11-02.
- **Kortix**: [kortix.com/docs/developers](https://kortix.com/docs/developers#kortixyaml-reference). The manifest reference: `policy.default_mode`, the `policies:` rule, and the grant rule that an omitted grant grants nothing. Read 2026-11-02.
- **Kortix**: [Kortix on GitHub](https://github.com/kortix-ai/suna). The product repository, where the harness that every session runs is documented. Read 2026-11-02.

## Codex

- **Codex**: [github.com/openai/codex](https://github.com/openai/codex). "Lightweight coding agent that runs in your terminal". Read 2026-11-02.
- **Codex**: [Agent approvals and security](https://learn.chatgpt.com/docs/agent-approvals-security). Sandbox mode and approval policy as two layers, the Auto preset, read-only mode, and the retired `untrusted` value. Read 2026-11-02.
- **Codex**: [Sample configuration](https://learn.chatgpt.com/docs/config-file/config-sample). `approval_policy`, `sandbox_mode`, `sqlite_home` as the runtime state directory, and `[history] persistence`. Read 2026-11-02.

## OpenCode

- **OpenCode**: [github.com/anomalyco/opencode](https://github.com/anomalyco/opencode). "The open source coding agent." Read 2026-11-02.
- **OpenCode**: [Permissions](https://github.com/anomalyco/opencode/blob/dev/packages/web/src/content/docs/permissions.mdx). The `permission` config, the three values allow, ask and deny, and the `--auto` flag. Read 2026-11-02.
- **OpenCode**: [Troubleshooting](https://opencode.ai/docs/troubleshooting). Session data at `~/.local/share/opencode/`, including `opencode.db`. Read 2026-11-02.

## Cline

- **Cline**: [github.com/cline/cline](https://github.com/cline/cline). "Autonomous coding agent as an SDK, IDE extension, or CLI assistant." Read 2026-11-02.
- **Cline**: [Auto Approve](https://github.com/cline/cline/blob/main/docs/features/auto-approve.mdx). Auto Approve evaluated per tool call, and the six categories it covers. Read 2026-11-02.
- **Cline**: [Checkpoints](https://docs.cline.bot/core-workflows/checkpoints). The shadow Git repository, a commit after each tool use, and checkpoints that persist across editor sessions. Read 2026-11-02.

## Aider

- **Aider**: [github.com/Aider-AI/aider](https://github.com/Aider-AI/aider). "aider is AI pair programming in your terminal". Read 2026-11-02.
- **Aider**: [aider/args.py](https://github.com/Aider-AI/aider/blob/main/aider/args.py). `--yes-always` and `--auto-commits`, with their own help text. Read 2026-11-02.
- **Aider**: [Git integration](https://aider.chat/docs/git.html). The commit per edit, "(aider)" in the author and committer, and `/undo`. Read 2026-11-02.

## OpenHands

- **OpenHands**: [github.com/OpenHands/OpenHands](https://github.com/OpenHands/OpenHands). "The self-hosted developer control center for coding agents and automations." Read 2026-11-02.
- **OpenHands**: [Security](https://docs.openhands.dev/sdk/guides/security). The confirmation policy, its three settings, and the security analyzer. Read 2026-11-02.
- **OpenHands**: [Resume conversations](https://docs.openhands.dev/openhands/usage/cli/resume). Conversation history in `~/.openhands/conversations`. Read 2026-11-02.

## What is not a source

A blog post is not a source, and neither is an aggregator list, a benchmark page, or a star count. Each of those is a claim about a project written by somebody who is not the project, and the table is only as good as the distance between a cell and the code it describes. A row that cannot be sourced from the project's own repository or documentation is deleted rather than filled with a guess.

## How to check a line

Open the URL, find the sentence the cell paraphrases, and compare the two. The cells quote the mechanism in the project's own terms, so a cell that reads like a summary is a cell that needs another look. If the page has moved, the monthly audit finds the new one on the project's own site and updates this file in the same change request as the row.
