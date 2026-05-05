---
description: 'YAML formatting standards'
applyTo: '**/*.{yaml,yml}'
---

# YAML Instructions

## YAML Content Rules

- Allow at most one space inside braces (yamllint `braces`).
- Be aware of potentially different formats embedded/stored in YAML strings (e.g. JSON strings).
  When modifying these values, ensure you do not break the embedded format's syntax
  (for example, avoid changing `"key": "value"` to `"key": >-` inside a JSON string, which would make it invalid JSON).
- End files with a newline (yamllint `new-line-at-end-of-file`).
- Keep lines <=120 characters (yamllint `line-length`).
- Keep list/map items in lexicographical order when practical.
- Limit consecutive blank lines to one (yamllint `empty-lines`).
- Use spaces for indentation (2 spaces per level from .editorconfig); avoid tabs.
- Use explicit booleans `true`/`false`; avoid other truthy values (yamllint `truthy`).
- When embedding code blocks or inline YAML snippets inside lists, include a trailing newline to preserve indentation.

## Validation

- Repository rules come from `.yamllint`.
- Run `pre-commit run yamllint -a` to verify locally.
- Use `yq` for parsing and validation of YAML files, especially for long or complex files: `yq . file.yaml`.
- [Find the right syntax for your YAML multiline strings](https://yaml-multiline.info/)
