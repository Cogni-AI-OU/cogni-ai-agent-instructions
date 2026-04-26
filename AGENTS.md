# AGENTS.md

Persistent single-source truth for autonomous agent behavior.

For general project invariants see [README.md](README.md).

## Directory-Specific Agent files

Read and merge these when operating inside corresponding sub-directories (order = precedence):

- [`.github/AGENTS.md`](.github/AGENTS.md)
- Any `AGENTS.md` or `SKILL.md` in ancestor, then current directory tree

## Required References

- Agent configuration & conventions: [copilot/copilot.instructions.md](copilot/copilot.instructions.md)
- Latest org baseline: <https://github.com/Cogni-AI-OU/.github/blob/main/AGENTS.md>
- Project overview & install: [README.md](README.md)
- Workflow navigation: [.tours/getting-started.tour](.tours/getting-started.tour)

## Instructions

You must load the instructions relevant to the files you are editing:

- **[ansible](ansible/ansible.instructions.md)**: Conventions, idempotency, and linting for Ansible content (Scope: Ansible roles and playbooks)
- **[blog](blog/blog.instructions.md)**: Blog post specific content standards and validation (Scope: docs/blog/**/*.md, blog/**/*.md, posts/**/*.md)
- **[copilot](copilot/copilot.instructions.md)**: Coding standards and project context (Scope: Copilot)
- **[github-workflows](github-workflows/github-workflows.instructions.md)**: Ordering, formatting, validation for GitHub Actions workflows (Scope: .github/workflows)
- **[json](json/json.instructions.md)**: Formatting rules for JSON and JSONC (Scope: **/*.json)
- **[markdown](markdown/markdown.instructions.md)**: Markdown structure and linting expectations (Scope: **/*.md)
- **[mermaid](mermaid/mermaid.instructions.md)**: Mermaid formatting standards, best practices, and anti-patterns (Scope: **/*.{md,mmd})
- **[readme](readme/readme.instructions.md)**: Layout, badges, and content guidance for the main README (Scope: Repository README.md)
- **[yaml](yaml/yaml.instructions.md)**: YAML formatting and linting rules (Scope: **/*.{yaml,yml})

## Common Tasks

### Linting and Validation

```bash
# Run all pre-commit checks
pre-commit run -a

# Run specific checks
pre-commit run markdownlint -a
pre-commit run yamllint -a
```

### Updating Coding Standards

- Update `.markdownlint.yaml`, `.yamllint`, or `.editorconfig` for linting rules
- Run `pre-commit run -a` to verify changes pass all checks

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
