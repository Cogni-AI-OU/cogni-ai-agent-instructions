# AGENTS.md

Persistent single-source truth for autonomous agent behavior.

For general project invariants see [README.md](README.md).

## Directory-Specific Agent files

Read and merge these when operating inside corresponding sub-directories (order = precedence):

- [`.github/AGENTS.md`](.github/AGENTS.md)
- [`.vscode/AGENTS.md`](.vscode/AGENTS.md) (command permissions and tasks)
- Any `AGENTS.md` or `SKILL.md` in ancestor, then current directory tree

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

### File operations

### Editing files

- When modifying or creating documentation and plain text files, always enforce line-wrapping and length
  limits in accordance with project-defined standards (such as `.markdownlint.yaml` or `.editorconfig`).

## Tooling

- Use MCP when possible.
- Use `pre-commit` for linting and validation if installed.
- For dumping links use `links -dump` if installed.

### Understanding the Task

- When the task is not clear, look for additional context.
- If triggered by a brief comment, check whether the parent comment exists and includes more detail.
- If it's still ambiguous, communicate with the user and propose options.


### Adding or Modifying Workflows

- Workflows in `.github/workflows/` can be reused via `workflow_call`
- Test workflow changes on a feature branch before merging to main
- Use `actionlint` to validate workflow syntax locally

## References

- Main documentation: [README.md](README.md)

## Troubleshooting

### Firewall issues

If you encounter firewall issues when using the GitHub Copilot Agent:

- Refer to <https://gh.io/copilot/firewall-config> for configuration details.
- Do not workaround blocked URLs by adding markdown-link-check ignore/whitelist patterns for real links.
- Keep markdown-link-check validating real links, and request firewall allowlisting instead.
- If you need to allowlist additional hosts, update your firewall configuration accordingly
  by following `.github/FIREWALL.md` and keep that file up to date.

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

- [README.md](README.md): Overview of instruction purpose and validation tooling
  (Scope: All instructions)
- [ansible.instructions.md](ansible/ansible.instructions.md): Conventions, idempotency, and linting for Ansible content
  (Scope: Ansible roles and playbooks)
- [blog.instructions.md](blog/blog.instructions.md): Blog post specific content standards and validation
  (Scope: docs/blog/**/*.md, blog/**/*.md, posts/**/*.md)
- [github-workflows.instructions.md](github-workflows/github-workflows.instructions.md): Ordering, formatting, validation for GitHub Actions workflows
  (Scope: .github/workflows)
- [json.instructions.md](json/json.instructions.md): Formatting rules for JSON and JSONC
  (Scope: **/*.json)
- [markdown.instructions.md](markdown/markdown.instructions.md): Markdown structure and linting expectations
  (Scope: **/*.md)
- [mermaid.instructions.md](mermaid/mermaid.instructions.md): Mermaid formatting standards, best practices, and anti-patterns
  (Scope: **/*.{md,mmd})
- [readme.instructions.md](readme/readme.instructions.md): Layout, badges, and content guidance for the main README
  (Scope: Repository README.md)
- [yaml.instructions.md](yaml/yaml.instructions.md): YAML formatting and linting rules
  (Scope: **/*.{yaml,yml})

### Usage

- Before editing a file, check this catalog and read the relevant instruction file.
- Follow the `applyTo` patterns in each instruction file to know when it applies.
- Keep this catalog updated when adding, renaming, or removing instruction files.
