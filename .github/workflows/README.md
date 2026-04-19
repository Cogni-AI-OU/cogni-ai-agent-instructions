# GitHub Actions Workflows

This directory contains GitHub Actions workflows and related configuration.
Reusable and repository workflows that automate checks, reviews, and AI-powered tasks.

- For the agent-facing workflow catalog, see [AGENTS.md](AGENTS.md).
- For editing guidelines, follow [github-workflows.instructions.md](../../github-workflows/github-workflows.instructions.md).

## Workflows

### Check Workflow (`check.yml`)

The `check.yml` workflow runs on pull requests, pushes, and weekly schedule to
ensure code quality and correctness.

Jobs:

- **actionlint**: Validates GitHub Actions workflow files
- **link-checker**: Checks for broken links in Markdown files using Lychee
- **pre-commit**: Runs pre-commit hooks for code formatting and linting

#### Link Checker

The link checker job uses [Lychee](https://github.com/lycheeverse/lychee) to
scan all Markdown files for broken links. It includes caching to avoid rate
limits and can be configured via `.lycheeignore` at the repository root to
exclude specific URLs or patterns.

**Local Testing**: You can test links locally with the configured
`markdown-link-check` pre-commit hook:

```bash
# Install from requirements.txt
pip install -r .devcontainer/requirements.txt

# Check a single file
pre-commit run markdown-link-check --files path/to/file.md

# Check all Markdown files
pre-commit run markdown-link-check -a
```

The hook uses `.markdown-link-check.json` and checks both local file references
and remote URLs before you push changes.

### Cogni AI Agent Workflow (`cogni-ai-agent.yml`)

The `cogni-ai-agent.yml` workflow provides the underlying logic to run the Cogni AI Agent. It runs on issue
comments, pull request review comments, and manual triggers (`workflow_dispatch`). It installs Python dependencies
from `.devcontainer/requirements.txt` and calls the `Cogni-AI-OU/cogni-ai-agent-action` to process natural language
instructions and automate repository tasks using selected LLM models.

### Copilot Setup Steps Workflow (`copilot-setup-steps.yml`)

The `copilot-setup-steps.yml` workflow is a utility workflow that checks out the repository, sets up Python 3.12,
restores the Python user site cache, and installs dependencies from `.devcontainer/requirements.txt`. It is triggered
on pushes and pull requests that modify the workflow file or the requirements file.

### Devcontainer CI Workflow (`devcontainer-ci.yml`)

The `devcontainer-ci.yml` workflow builds and tests the development container image.

**Purpose**: It ensures that all required command-line tools (e.g., `docker`, `gh`, `pre-commit`) and Python packages
(e.g., `ansible-lint`, `molecule`) are properly installed and functional inside the devcontainer. It runs on changes
to the `.devcontainer` directory, on a weekly schedule, and can also be used as a reusable workflow (requires
`packages: write` permission for the caller).

## Problem Matchers

GitHub Actions problem matchers automatically annotate files with errors and
warnings in pull requests, making it easier to identify and fix issues.

### Available Matchers

- **actionlint-matcher.json**: Captures errors from actionlint workflow linting
- **pre-commit-matcher.json**: Captures errors from pre-commit hooks

### Pre-commit Problem Matcher

The pre-commit problem matcher supports two output formats:

1. **Generic format** (`file:line:col: message`): Used by flake8, actionlint,
   and other tools that provide column information
2. **No-column format** (`file:line message`): Used by markdownlint and other
   tools that only provide line numbers

Note: Some hooks like yamllint and ansible-lint already output GitHub Actions
annotations directly and don't need the problem matcher.

### Configuration

Problem matchers are registered in the `.github/workflows/check.yml` workflow
before running the corresponding tools.

## Using these workflows

- Reference a workflow from another repo with `uses: Cogni-AI-OU/.github/.github/workflows/<file>@main`.
- Consult the catalog in [AGENTS.md](AGENTS.md) for inputs, triggers, and job details.
- Keep branch protection and required checks enabled when consuming workflows that can push commits.

## Security

### Cogni AI Agent Workflow Git Access

The Cogni AI Agent workflow (`cogni-ai-agent.yml`) grants intentionally broad git access
to enable autonomous code changes. This permission is necessary for the agent to commit and
push changes, but requires proper safeguards.

#### Security Controls

**Access Control:**

- Only trusted users can trigger the agent (OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR)
- PR/issue authors can only trigger on their own content
- External contributors (FIRST_TIME_CONTRIBUTOR, NONE) are explicitly blocked

**Required Repository Protections:**

To safely use the Cogni AI Agent with git access, repository administrators must configure:

1. **Branch Protection Rules** on main/protected branches:
   - Require pull request reviews before merging
   - Require status checks to pass (e.g., linting, tests)
   - Require conversation resolution before merging
   - Do not allow bypassing the above settings

2. **GitHub Audit Logs** (organization-level):
   - Enable and regularly review audit logs
   - Monitor commits made by `github-actions[bot]` (the agent's identity)
   - Set up alerts for suspicious patterns (rapid commits, deleted branches, etc.)

3. **Protected Branch Policies**:
   - Restrict who can push to protected branches
   - Consider requiring deployment approvals for production branches
   - Use CODEOWNERS to require specific reviewer approval for sensitive files

#### Best Practices

- Review the agent's commits before merging PRs
- Use draft PRs for the agent's work to require explicit promotion
- Regularly audit the agent's tool usage and permissions
- Rotate `OPENCODE_API_KEY` periodically
- Monitor workflow run logs for unexpected behavior
