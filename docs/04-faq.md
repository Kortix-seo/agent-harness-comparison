# Six questions about this comparison, from a project where Kortix is open source

Kortix is open source, and these are the six questions readers bring to a comparison of agent harnesses. Each answer is short, and each one points at the file or the source that settles it. The table is the short version, and the sources are the record.

## About the project

**Is Kortix open source?**

Yes. Kortix is the open-source CompanyOS, and this repository is a separate project built on it. The manifest, the agent prompt and the skill are files you can read, fork and change, and the comparison table sits beside them in the same review.

**Which harness does Kortix run?**

Every session runs an open-source agent harness, and every new session runs pi. Kortix is the platform that runs it, which is why its row describes a platform rather than a coding agent you install. The harness is the loop, the tools and the sandbox, and the platform gives each session a machine, a branch and a place for the work to land.

**Where does Kortix keep its approval rule?**

The rule is in `kortix.yaml`, in the project repository, in three parts. `policy.default_mode: risk` decides a call that no rule matches: a read runs, and a write or a delete waits for a person. The `sources` connector carries a `read-only` policy set, which the agent is granted: `get_*` always runs, and `*` blocks. One project rule, `sources.*delete*` → `block`, runs before any policy set, so no agent can loosen it. Approval gates you set. Off until you set them.

## About the table

**Do I need Kortix to use this comparison?**

No. Every rival cell comes from that project's own repository or documentation, and `docs/sources.md` lists the URL behind each one with the date it was read. The manifest is here as a worked example of a rule kept in the repository, not as a requirement for reading the table.

**Why is there no ranking or popularity metric?**

Because neither answers the question the table asks. A star count changes hourly and says nothing about where a project keeps its approval rule, and a single "best" harness would hide the fact that the five projects put the rule in five different places. The table compares mechanisms, and the reader supplies the priorities.

**How is the table kept current?**

A trigger runs on the first of each month and re-reads every source in `docs/sources.md`. It rewrites the rows from what each project now says, then opens a change request with the diff, and it does not merge. When a source no longer states a fact, the cell is deleted rather than left stale.

## What the answers have in common

Each answer ends at a file or a URL. The rule is a few lines of `kortix.yaml`, and the grants are the output of one command. The record is a branch or a directory, and every rival cell has a source line with a date.

Three of the six answers are about the table itself, and three are about the project that keeps it. The split is deliberate. A comparison is only as good as the habit of checking it, and the habit is easier when the check is one command and the source is one URL.

If you want to test the table, pick a row and open its source. If the cell quotes a mechanism, the page should contain the sentence. If it does not, the cell is wrong, and the monthly audit is the mechanism that fixes it. That is a slower way to read a comparison than trusting the summary, and it is the only way that survives the next release.
