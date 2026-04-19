# GitHub Actions Workflows (Agent Catalog)

Authoritative, agent-facing catalog of workflows in this repository. Use this when loading or modifying
workflows and keep it in sync with the files in this directory.

For a human-readable overview, see [README.md](README.md).

## Workflow catalog

- **[check.yml](check.yml)**: Linting and quality gates via actionlint and pre-commit.
- **[cogni-ai-agent.yml](cogni-ai-agent.yml)**: Cogni AI Agent for autonomous repository tasks.
- **[copilot-setup-steps.yml](copilot-setup-steps.yml)**: Utility for setting up Python and dependencies.
- **[devcontainer-ci.yml](devcontainer-ci.yml)**: Build/test devcontainer and required tools/packages.

## Details

### check.yml

- Purpose: run actionlint and pre-commit to enforce workflow and repo standards.
- Triggers: `push`, `pull_request`, `schedule`, `workflow_call`, `workflow_dispatch`,
  `workflow_run` (after bot workflow completions).
- Bot-PR support: `workflow_run` trigger enables checks on PRs created by bots,
  since normal `pull_request` events don't trigger for bot actors.
- Reusable: `uses: Cogni-AI-OU/.github/.github/workflows/check.yml@main`.
- Jobs: `actionlint`, `link-checker`, `pre-commit`.

### cogni-ai-agent.yml

- Purpose: run the Cogni AI Agent to process natural language instructions and automate tasks.
- Triggers: `issue_comment`, `pull_request_review_comment`, `workflow_dispatch`.
- Inputs: `model` (choice of LLM), `prompt` (task description).
- Permissions: `contents: write`, `id-token: write`, `issues: write`, `pull-requests: write`.

### copilot-setup-steps.yml

- Purpose: setup Python 3.12, cache, and install dependencies from `.devcontainer/requirements.txt`.
- Triggers: `push`/`pull_request` on requirements or workflow; `workflow_dispatch`.

### devcontainer-ci.yml

- Purpose: build and validate the dev container; ensure required tools and Python packages exist.
- Inputs: `required_commands` (defaults to common CLI tools), `required_python_packages`
  (defaults to ansible, ansible-lint, docker, molecule, pre-commit, uv).
- Triggers: pull_request/push affecting `.devcontainer/` or this workflow; weekly schedule;
  `workflow_call`.
- Permissions: callers must grant `packages: write` when pushing images to GHCR.
- Reusable: `uses: Cogni-AI-OU/.github/.github/workflows/devcontainer-ci.yml@main`.

## Notes

- Follow [github-workflows.instructions.md](../../github-workflows/github-workflows.instructions.md)
  when editing workflow files (ordering, formatting, validation).
- Keep this catalog updated when workflows are added, removed, or renamed.
