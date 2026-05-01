# AGENTS-RUNTIME.md

Persistent single-source truth for autonomous agent behavior.

## Instructions

You must load the instructions relevant to the files you are editing:

- **[ansible](ansible/ansible.instructions.md)**: Conventions, idempotency, and linting for Ansible content
  (Scope: Ansible roles and playbooks)
- **[blog](blog/blog.instructions.md)**: Blog post specific content standards and validation
  (Scope: docs/blog/**/*.md, blog/**/*.md, posts/**/*.md)
- **[github-workflows](github-workflows/github-workflows.instructions.md)**: Ordering, formatting, validation for GitHub Actions workflows
  (Scope: .github/workflows)
- **[json](json/json.instructions.md)**: Formatting rules for JSON and JSONC
  (Scope: **/*.json)
- **[markdown](markdown/markdown.instructions.md)**: Markdown structure and linting expectations
  (Scope: **/*.md)
- **[mermaid](mermaid/mermaid.instructions.md)**: Mermaid formatting standards, best practices, and anti-patterns
  (Scope: **/*.{md,mmd})
- **[readme](readme/readme.instructions.md)**: Layout, badges, and content guidance for the main README
  (Scope: Repository README.md)
- **[yaml](yaml/yaml.instructions.md)**: YAML formatting and linting rules
  (Scope: **/*.{yaml,yml})

### Structural Invariant

- **Instructions Location**: Instructions are located directly in the root directory of this repository.
