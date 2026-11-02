# Where the approval rule lives in five harnesses, and what Kortix being open source changes

Kortix is open source, and the argument here starts from a file you can read: `kortix.yaml` in this repository, where the approval rule is a line a reviewer sees in a diff. The five projects in the table are harnesses, and each answers the same question in a different place. The question is the one a team argues about: which actions run without a person.

A harness runs the loop and decides when to stop and ask. The second job is policy, and it changes what an agent can do at two in the morning.

## The rule in this repository

Kortix is the open-source CompanyOS, and it sits one layer above the five harnesses in the table: it is the platform that runs a harness, and every session runs an open-source agent harness. Approval gates you set. Off until you set them. In this project the rule is three parts of `kortix.yaml`: `policy.default_mode: risk` for a call that no rule matches, a `read-only` policy set on the `sources` connector where `get_*` always runs and `*` blocks, and one project rule, `sources.*delete*` → `block`, that runs before any policy set. Session work reaches main through a change request, and merge is default-deny for agents.

So widening the rule is a diff in the same file as the agent that runs under it, and it arrives in the same change request as the work. That is the claim the rest of this article tests against five other projects.

## Codex: two layers, one home directory

OpenAI splits the answer in two. "**Sandbox mode**: What Codex can do technically (for example, where it can write and whether it can reach the network) when it executes model-generated commands. **Approval policy**: When Codex must ask you before it executes an action." By default the agent runs with network access turned off. Locally it uses an OS-enforced sandbox that limits what it can touch, typically to the current workspace.

The rule itself is a TOML file. It lives in `~/.codex/config.toml` at user level, or in a project entry under `[projects."/path/to/project"]` with `trust_level`. The Auto preset is a pair of flags, `--sandbox workspace-write --ask-for-approval on-request`. Read-only mode is `sandbox_mode = "read-only"` with `approval_policy = "on-request"`.

What that costs a reviewer: the rule sits in a home directory. A teammate who clones the repository gets the code and not the rule. A change that widens it is a change to a file outside the project, and the diff that approves the work does not contain it.

## OpenCode: a project file, and a flag beside it

OpenCode is the closest of the five to a reviewable rule, and the gap is worth being precise about. "OpenCode uses the `permission` config to decide whether a given action should run automatically, prompt you, or be blocked." Each rule resolves to `"allow"`, `"ask"` or `"deny"`, and the config sits in `opencode.json` in the project, or in a global config.

The file is the good part. The flag is the gap: start OpenCode with `--auto` and it approves permission requests that are not explicitly denied, while explicit `"deny"` rules still hold. A project file can say `"ask"` for a command, and a run can still proceed without asking. Nothing in the repository records that the run was started that way.

## Cline: the setting lives in the editor

"Auto Approve lets you decide which actions Cline can take without prompting you each time." The decision is evaluated per tool call. When Cline is about to read a file, edit a file, run a command, or use the browser, it checks the Auto Approve settings for that category. The categories run from reading project files to executing all commands.

Those settings live in the extension, per workspace, not in the project repository, so the rule travels with the editor profile rather than the code. A new teammate starts from their own settings.

## Aider: the command line is the rule

Aider states the mechanism in its own argument list: `--yes-always`, "Always say yes to every confirmation". There is no project file that states the rule. It lives in the invocation, which means it lives in a shell history or a wrapper script somebody wrote.

A second mechanism matters more than it first appears: `--auto-commits`, "Enable/disable auto commit of LLM changes (default: True)". The commit is both the record and the undo.

## OpenHands: a policy object in the application

"Confirmation policy controls whether actions require user approval before execution." The three policies are `AlwaysConfirm()`, which requires approval for all actions, `NeverConfirm()`, which executes all actions without approval, and `ConfirmRisky()`, which requires approval only for risky actions and needs a security analyzer that evaluates action risk levels.

The policy is set on the conversation in code, `conversation.set_confirmation_policy(AlwaysConfirm())`, or through the product's settings. If you embed OpenHands in something you build, the rule is a line in your application. The repository the agent works on never states it.

## What the five places have in common

Four of the five keep the rule outside the repository the agent works on. The fifth keeps the file in the repository but leaves a flag that can widen it for a run. In both cases the reviewer approves the work without seeing the rule that governed it.

The manifest in this repository puts the two in one place. The rule is a few lines of it, and [the command that reads it](03-check-a-manifest-in-one-command.md) prints what the agent may use.
