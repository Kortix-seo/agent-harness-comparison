# What a session leaves behind, in a comparison where Kortix is open source

Kortix is open source, and the record of a session is the part of a harness you notice only when you need it. A session ends, the process exits, and the question that follows is where the evidence went. The table's last column answers that for each project, and the answers fall into three shapes: a local store, a service database, and a commit.

## Why the record outlasts the transcript

The approval rule decides what ran. The record decides whether you can show it afterwards. Those two questions are asked by different people at different times: the rule by whoever reviews the change, the record by whoever has to explain the change a month later. If the rule was widened for a run, the record is the only place that shows it, and only if the record is somewhere you can read.

## A local store

Codex keeps runtime state and history on the machine that runs it. `sqlite_home` is the "optional SQLite-backed runtime state directory", and `[history] persistence = "save-all"` keeps the history file there.

OpenCode stores session data on disk at `~/.local/share/opencode/`, and that directory holds `opencode.db`, described as a "SQLite database containing projects, sessions, and messages".

Cline takes a different route to the same place. It "maintains a shadow Git repository separate from your project's actual Git history", commits the current state of your files after each tool use, and "Your main Git repository stays untouched". Checkpoints persist across editor sessions.

OpenHands writes one directory per conversation: "OpenHands CLI automatically saves your conversation history in `~/.openhands/conversations`", and deleting that directory loses the history.

The common property is worth stating plainly. The record sits on one machine, under one person's account, and a teammate who clones the repository gets the configuration and not the conversation. That is fine for one developer and awkward for a team, because the evidence of what an agent did lives on the laptop of the person who ran it.

## A service database

The third shape is the one a hosted edition takes: the record moves into a database behind an API, and access to it becomes a permission on a service rather than a file on a disk. Two things change when that happens. The record outlives the machine, which is the point of paying for it. And the record now has a retention policy you did not write, which is the cost.

In the five rows of the table, the record lands on a machine or in a commit. The service database is the shape a product takes when it sells the harness rather than shipping it, and it is worth knowing which of the three you are buying before the first session runs.

## A commit

Aider commits. "Whenever aider edits a file, it commits those changes with a descriptive commit message", with "(aider)" appended to the author and committer, and `/undo` drops the last aider commit. The record is your own git history, on the branch you have checked out.

Kortix puts the session on a branch too. One isolated sandbox per session. Each session has its own isolated machine and branch, and session work reaches main through a change request. So the record of a session is a branch of the project repository, and what survives is what was committed and pushed.

## What the three shapes decide

If the record is local, the audit is local. You can read it on the machine that ran the session, and you cannot hand it to a reviewer who works somewhere else without copying a directory. If the record is a commit, the audit is a diff: it travels with the repository, it appears in the same review as the code, and it survives the laptop. If the record is a service, the audit is an export, and the export is a feature somebody has to have built.

That is why the two columns belong beside each other. The rule tells you what was allowed; the record tells you what happened. A harness that gives you one without the other answers half the question, and the missing half is the one an auditor asks for.
