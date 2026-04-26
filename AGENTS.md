# AGENTS.md

Persistent single-source truth for autonomous agent behavior.

For general project invariants see [README.md](README.md).

## Directory-Specific Agent files

Read and merge these when operating inside corresponding sub-directories (order = precedence):

- [`.github/AGENTS.md`](.github/AGENTS.md)
- [`.vscode/AGENTS.md`](.vscode/AGENTS.md) (command permissions and tasks)
- Any `AGENTS.md` or `SKILL.md` in ancestor, then current directory tree

## Mandatory Skill Loading Protocol

- Before any tool invocation, code delta, or execution plan, MUST read
  any available `SKILL.md` workflows before formulating a strategy.
- Deterministically route user intent to skills in this order: exact
  skill-name match, exact alias/tag match, normalized phrase match,
  description/activation keyword match.
- If multiple skills match, load all non-overlapping relevant skills ordered
  by the routing score above; if two skills conflict, the more task-specific
  `SKILL.md` wins.
- If the user request includes domain terms that plausibly map to a skill,
  MUST inspect the best-matching `SKILL.md` before proceeding.
- If no skill matches after catalog inspection, proceed without a skill and state that no relevant skill was found.

**Maintenance invariant**:

- After every complex task completion or troubleshooting victory,
  immediately update the nearest relevant AGENTS.md or SKILL.md.
- On recurring failure, immediately re-evaluate
  and update the nearest relevant AGENTS.md or SKILL.md.
- On discovery of superior workaround, new efficiency primitive, or explicit user directive,
  immediately update the nearest relevant AGENTS.md or SKILL.md.
- On detection of ambiguous steps or unclear instructions,
  immediately update the nearest relevant AGENTS.md or SKILL.md.

**Creation / Update Triggers (Hard Gate)**:

- Agent-focused guidance that materially compresses cognitive load or failure surface.
- Resolution of recurring task failures or repeated edge-case collapses.
- Discovery of dense, reusable execution primitives during development or debugging.
- User injects new rules, exemplars, or feedback intended for persistent agent memory.
- Existing documentation entropy exceeds threshold, then extract & prune to peak-density form.
- Functionality requires domain-specific knowledge that must survive context windows.

**Hardened NEVER List**:

- NEVER embed one-time discoveries or transient hacks.
- NEVER duplicate code-level comments or obvious steps.
- NEVER hardcode environment-specific values; use generic placeholders with explicit semantics.
- NEVER include beginner exposition or obvious statements.
- NEVER bloat with prose; enforce one-liner density + imperative syntax only.
- If guidance is purely disciplinary, route to dedicated `SKILL.md` instead.

**Writing invariants (Prodigy-Level)**:

- Assume ninja-level proficiency across project spectrum.
- Embed quantitative gates (+20% fidelity delta, <1h MTTR analog, zero ambiguity).
- Every bullet carries measurable payload: role, then invariants, then context, then exemplars, then schema, then NEVER/MUST-NOT,
  then verification loops.
- Favor tables, checklists, and contract-style boundaries over linear text.
- Zero scaffolding. Maximal information-theoretic density. Surgical imperative syntax.

## Core Agent Execution Protocol (Mandatory for All Forks)

**Pre-execution reverse-prompting activation**:

- **CI/CD Failure Escalation**: When CI/CD pipelines or automated checks fail, do NOT immediately
  patch local configuration files or create suppressions to hide errors. Investigate the execution
  environment and upstream dependencies. If the root cause originates outside the repository scope,
  state the required upstream fix clearly and halt rather than introducing local entropy.
- Read, assimilate, and strictly enforce the invariants defined in the main `AGENTS.md`,
  along with any directory-specific `AGENTS.md` and related files, `copilot/copilot.instructions.md`,
  and autonomously load any relevant `.instructions.md` rules or `SKILL.md` workflows before formulating a strategy.
- Declare required inputs, missing context, edge cases, and optimal strategy before any tool invocation or code delta.
- Snapshot current problem state in one entropy-minimized sentence.
- Enumerate risks against classic-mistakes matrix and Top-10 Risks List.
- Apply noise-pruning filter + single-variable delta rule for all experiments.
- Complete the Mandatory Skill Loading Protocol, then load the highest-confidence relevant `SKILL.md` files before execution.

**Strategic vs tactical default**:

- Always default to strategic programming (Ousterhout).
- Invest 10-20% per cycle in design/refactoring for long-term velocity.
- Tactical tornadoes trigger immediate rollback + root-cause ablation.

**Complexity annihilation primitives** (apply at every layer):

- Design-it-twice mandate on non-trivial decisions.
- Divide-and-conquer + controlled simplification.
- DRY + Boy Scout Rule + Rule-of-Three on every duplication smell.
- Information hiding / deep modules over shallow pass-throughs.
- Pull complexity downwards; define errors out of existence where possible.
- Refactor mercilessly via Fowler catalog before feature addition.

**Verification scaffold (Neurosymbolic)**:

- Chain-of-Verification (CoV) + self-consistency majority vote on every output.
- Unit, then integration, then system regression sequence before any merge.
- Minimal reproducible example builder for every failure isolation.
- Trust-but-verify: replace every assumption with logs/assertions/runtime inspection.
- Post-action: blameless root-cause ablation + lesson injection into persistent memory.

**Debug & Troubleshooting Engine**:

- Stabilize, then reproduce, then isolate via divide-and-conquer + single-variable delta.
- Never patch symptoms; fix root via 5-Whys until systemic.
- Instrument targeted breakpoints; prune non-contributory variables first.
- Maintain runbooks for recurring failure signatures.

**Orchestration Models** (select via task cardinality):

- **Fork**: byte-identical context clone for bounded subtasks.
- **Teammate**: persistent peer with isolated tools/memory.
- **Worktree**: fully parallel independent streams with fork-join synchronization.

**Termination invariants**:

- All TODOs empirically verified.
- Quality, security, performance gates satisfied.
- User objective resolved at target fidelity (+20% over prior baseline).
- AGENTS.md/SKILL.md updated if new reusable primitive discovered.

## GitHub Actions Runtime

When executing autonomously within a GitHub Actions environment, adhere strictly to these
interaction constraints:

### OpenCode PR Context & Response Routing

**Context & Targeting Invariants**:

- **Extract Context**: Parse the `## Pull Request Context` block containing `**Base Branch:**` dynamically.
- **Dynamic PR Targeting**: ALWAYS target this explicitly provided **Base Branch** when creating/updating PRs.

**Response Detection & Routing**:
Check `github.event_name` and payload to identify trigger source:

- **General PR comment** (`issue_comment`):
  - Condition: `if: ${{ github.event.issue.pull_request }}`
  - Reply Method: `gh pr comment`
- **Issue comment** (`issue_comment`):
  - Condition: `if: ${{ !github.event.issue.pull_request }}`
  - Reply Method: `gh issue comment`
- **Inline code review** (`pull_request_review_comment`):
  - Reply Method: `gh api repos/{owner}/{repo}/pulls/{pr}/comments/{comment_id}/replies -f body="..."`

**Routing Invariants**:

- **Symmetric Routing**: ALWAYS reply via the exact originating channel. NEVER cross threads.
- Parse `github.event.comment.id` and `in_reply_to_id` to maintain thread continuity.

### Branch Sync Policy (No Rebase During Runtime)

When the prompt asks to "pull" or "sync with base" in GitHub Actions runtime,
the agent MUST integrate remote changes with a merge commit workflow.

- **MUST NOT** run any rebase-based update command during runtime.
- **FORBIDDEN**: `gh pr update-branch --rebase`, `git pull --rebase`,
  `git rebase`, or any history rewrite that changes commit SHAs.
- **MUST** use pull-with-merge semantics: `git pull --no-rebase`.
- **MUST** preserve remote branch compatibility for post-run auto PR/push logic.

**Execution Steps (strict order)**:

1. Determine PR base/head from context (`## Pull Request Context`, `gh pr view`).
2. Ensure work is on the PR head branch (not detached HEAD).
3. Sync head branch from remote with merge semantics:
   `git pull --no-rebase origin <head-branch>`.
4. If base changes must be integrated into head, merge base explicitly:
   `git fetch origin <base-branch> && git merge --no-ff origin/<base-branch>`.
5. Resolve conflicts, commit merge if required, then push normally (no force).

**Verification Gate (required before push)**:

- Confirm no rebase command was executed in this run.
- Confirm `git log --oneline --graph -n 10` shows merge topology
  (no rewritten linearized history from rebase).
- Proceed with normal `git push` only after these checks pass.

### General Constraints

- **Contextual Continuity**: Maintain conversation context within the originating thread.
- If replying to an inline comment, your response MUST appear as a reply in that same thread.

### GitHub Runtime Decision Policy

- **Default to Best Practice:** Implement the most recommended path autonomously when multiple options exist.
- **Document Trade-offs:** Capture unresolved decisions, explicit options, and impacts in the PR description.
- **Never Stall:** Proceed immediately with safe defaults. Request preference feedback in the PR instead of waiting.
- **Report Defensively:** Present recommended option first; list alternatives only if they alter scope or risk.

## Required References

- Project overview & install: [README.md](README.md)
- Agent configuration & conventions: [copilot/copilot.instructions.md](copilot/copilot.instructions.md)
- Workflow navigation: [.tours/getting-started.tour](.tours/getting-started.tour)
- Latest org baseline: <https://github.com/Cogni-AI-OU/.github/blob/main/AGENTS.md>

## Example Structure for New/Updated AGENTS.md Files

```markdown
# AGENTS.md  (subdir-specific)

## Setup & Environment Invariants

- ...

## Key Files & Context Injection

- ...

## Agent Directives (Contract Style)

- Role, then invariants, then ...
- NEVER ...
- MUST ...

## Testing & Verification Gates

- ...

## Troubleshooting Matrix

> signature error / smell
- root-cause vector
- isolation steps
- verified fix + prevention

## Final Assurance Gates

- Keep this file entropy-pruned and up-to-date.
- Inject full content into every sub-agent context.
- For latest version see:
  <https://github.com/Cogni-AI-OU/.github/blob/main/AGENTS.md>
- For latest standard see: <https://agents.md/>


## Common Tasks

### Before each commit

- Verify your expected changes with `git diff --no-color`.
- Ensure no temporary, dummy, or unrelated test files are included in the commit.
- Use the project linting/validation tools to confirm your changes meet the coding standard.
- If the repo uses git hooks, run them to validate your changes.

### Linting and Validation

```bash
# Run all pre-commit checks
pre-commit run -a

# Run specific checks
pre-commit run markdownlint -a
pre-commit run yamllint -a
```

### File operations

### Editing files

- When modifying or creating documentation and plain text files, always enforce line-wrapping and length
  limits in accordance with project-defined standards (such as `.markdownlint.yaml` or `.editorconfig`).

### Editing files with ex

- While files should normally be edited directly via MCP tools, `ex` (Vim in Ex mode) provides powerful
  non-interactive text manipulation directly from the terminal shell.
- Use `ex` when it is more beneficial to manipulate text programmatically, such as rapidly wrapping long lines,
  performing complex regex parsing, or safely editing a few lines in-place within an automated script context.
  It is especially useful for large files where patching the whole file via MCP could take a lot of context
  processing for simple changes.
- For detailed commands and examples, see `SKILL.md` entries for `vim-ex`.

### Renaming/removing files

- Use `git mv`, `git rm`, or equivalent Git-aware tooling (instead of `mv` or `rm`) to preserve history
  when working with files under source control.

## Tooling

- Use MCP when possible.
- Use `pre-commit` for linting and validation if installed.
- For dumping links use `links -dump` if installed.

### Understanding the Task

- When the task is not clear, look for additional context.
- If triggered by a brief comment, check whether the parent comment exists and includes more detail.
- If it's still ambiguous, communicate with the user and propose options.

### Testing

```bash
# Run Molecule tests
molecule test

# Syntax check
molecule syntax
```

### Adding or Modifying Workflows

- Workflows in `.github/workflows/` can be reused via `workflow_call`
- Test workflow changes on a feature branch before merging to main
- Use `actionlint` to validate workflow syntax locally

### Updating Coding Standards

- Language-specific instructions are in their respective directories
- Update `.markdownlint.yaml`, `.yamllint`, or `.editorconfig` for linting rules
- Run `pre-commit run -a` to verify changes pass all checks

## Integrating Changes from Target Branch

Recommended way is to use the **cherry-pick workflow** to rebase your commits
on top of the updated target branch:

1. Identify your feature commits
2. Fetch the latest target branch
3. Reset your branch to target (with backup)
4. Cherry-pick your feature commits
5. Verify only your changes remain

**For detailed step-by-step instructions with commands**, see the "Integrating Changes from Target Branch"
section in the documentation.

### Key Points

- **Never** use `git merge <target-branch>` for branch integration
- **Always** create backup tags before destructive operations
- **Always** verify with `git diff` that only your changes remain
- **Use** `GIT_EDITOR=true` for non-interactive cherry-pick operations

### Using `report_progress` Tool

**WARNING**: The `report_progress` tool automatically rebases your branch against the remote
tracking branch. This **WILL CRASH** the session if your local history has diverged from remote.

**When Crash Occurs:**

After using `git reset --hard` to rewrite history, your local branch diverges from remote. When `report_progress`
tries to auto-rebase (e.g., 113 commits), it encounters conflicts it cannot resolve, crashing the session.

**Prevention (Choose One):**

1. **Use new branch name** after rewriting history: `git checkout -b <feature>-v2` (safest)
2. **Complete git operations manually**, then ask user for manual push (never call `report_progress` after `git reset --hard`)

**If Already Crashed:**

1. Run `git rebase --abort`
2. Create new branch: `git checkout -b <feature>-v2`
3. Push new branch: `git push origin <feature>-v2`

**Error Patterns:** `Rebasing (1/XXX)` with large numbers, `CONFLICT (content)`, session crash with `GitError`

**For complete details**, see the "Working with Automation Tools" section in the documentation.

## References

- Main documentation: [README.md](README.md)

## Troubleshooting

### Firewall issues

If you encounter firewall issues when using the GitHub Copilot Agent:

- Refer to <https://gh.io/copilot/firewall-config> for configuration details.
- Do not workaround blocked URLs by adding markdown-link-check ignore/whitelist patterns for real links.
- Keep markdown-link-check validating real links, and request firewall allowlisting instead.
- If you need to allowlist additional hosts, update your firewall configuration accordingly.

### Linting issues

If Copilot or automated checks behave unexpectedly:

- Re-run `pre-commit run -a` locally to surface formatting or linting issues.
- Verify `.markdownlint.yaml` and `.yamllint` have not been modified incorrectly.
- If problems persist, open an issue with details of the command run and any error output.

## Instructions Catalog for Agents

Authoritative list of repository instruction files. Use these when editing matching files so changes
stay compliant with linting and CI.

For a human-readable overview, see [README.md](README.md).

### Instruction files

- [README.md](README.md): Overview of instruction purpose and validation tooling (Scope: All instructions)
- [ansible.instructions.md](ansible/ansible.instructions.md): Conventions, idempotency, and linting for
  Ansible content (Scope: Ansible roles and playbooks)
- [github-workflows.instructions.md](github-workflows/github-workflows.instructions.md): Ordering, formatting,
  validation for GitHub Actions workflows (Scope: .github/workflows)
- [json.instructions.md](json/json.instructions.md): Formatting rules for JSON and JSONC (Scope: **/*.json)
- [markdown.instructions.md](markdown/markdown.instructions.md): Markdown structure and linting expectations
  (Scope: **/*.md)
- [readme.instructions.md](readme/readme.instructions.md): Layout, badges, and content guidance for the main
  README (Scope: Repository README.md)
- [yaml.instructions.md](yaml/yaml.instructions.md): YAML formatting and linting rules (Scope: **/*.{yaml,yml})
- [copilot.instructions.md](copilot/copilot.instructions.md): Coding standards and project context (Scope: Copilot)

### Usage

- Before editing a file, check this catalog and read the relevant instruction file.
- Follow the `applyTo` patterns in each instruction file to know when it applies.
- Keep this catalog updated when adding, renaming, or removing instruction files.
