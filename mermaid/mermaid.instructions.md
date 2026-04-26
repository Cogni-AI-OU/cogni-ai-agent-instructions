---
description: 'Mermaid formatting standards, best practices, and anti-patterns'
applyTo: '**/*.{md,mmd}'
---

# Mermaid Instructions

## Core Principles (Invariants)

- **Clarity First + Minimalism**: Only essential nodes/edges; avoid clutter.
  Diagrams must be readable at a glance.
- **Consistent Styling**: Prefer `classDef` / `class` over inline styles; use
  `%%` comments for clarity.
- **Doc-Rot Prevention**: Keep diagrams in the same Markdown as code/docs so
  they stay in sync.
- **Render-Portability**: All examples tested for GitHub + Mermaid Live Editor.
  Use standard syntax; quote strings containing spaces/special chars.
- **Testability (Tracer-Bullet Rule)**: Always recommend testing in
  [Mermaid Live Editor](https://mermaid.live/). Generate → test → refine.
- **Version Compatibility**: Target Mermaid v11+ (current stable). Note beta
  features separately.

## Best Practices (Agent Directives)

- **Design-it-Twice**: Generate two alternative layouts; pick the clearest.
- **ETC Mandate**: Name nodes descriptively; use aliases; avoid hard-coded
  coordinates.
- **Exhaustive Enumeration**: Cover all key entities before connecting.
- **Minimal Reproducible Example**: Test smallest viable diagram first.
- **Single-Variable Delta**: Change one node/edge/style at a time when
  iterating.
- **Verification Protocol**:
  1. Generate diagram.
  2. Paste into Mermaid Live Editor.
  3. Verify rendering on GitHub preview.
  4. Check for parser errors (unquoted strings, bad indentation).
  5. Confirm no "doc-rot" risk (diagram lives with code).

## What to Avoid (Hardened)

- **Assuming Beta Syntax**: Do not guess syntax for stable diagrams based on
  beta patterns. Always consult the latest official Mermaid documentation, as
  beta syntaxes can evolve.
- **Double Double-Quotes**: Do not use `[""...""]`. Mermaid expects exactly one
  pair of double quotes `["..."]`.
- **Frontmatter Comments Before Target**: NEVER place `%%` comments before the
  `---` YAML frontmatter block. Frontmatter config must be the absolute first
  lines inside the ```mermaid block.
- **Hardcoding Styles**: Prefer reusable `classDef` over inline styles.
- **Inconsistent Indentation**: Critical for Mindmap, TreeView, and
  hierarchical diagrams.
- **Keywords as IDs**: Avoid using `end` as node ID without quotes (flowchart
  pitfall).
- **Mindmap Icon Placement**: When attaching `::icon(...)` to a mindmap node,
  place it on the exact subsequent line with the EXACT same indentation as the
  node declaration to avoid parser ambiguity.
- **Mindmap Implicit Nodes**: NEVER use implicit text nodes (e.g., just
  `"Label"`) in mindmaps. ALWAYS explicitly map every node to an ID (e.g.,
  `node_id["Label"]`), especially for child nodes.
- **Nested Quotes in Pipes**: NEVER use raw double quotes (`"`) inside
  pipe-delimited edge labels (e.g., `-->|"Label"|`). This crashes many standard
  renderers. Use the HTML entity `&quot;` instead: `-->|&quot;Label&quot;|`.
- **Non-ASCII Characters**: Avoid typographic characters like `→` (right arrow)
  or `–` (en dash) in labels; use standard ASCII `->` or `-` to prevent
  `require-ascii` pre-commit failures.
- **Over-complexity**: Avoid massive diagrams (> 50 nodes) that become
  unreadable.
- **Quotes in Pipe Labels**: Never use double quotes `"` inside pipe-delimited
  labels (e.g., `-->|"text"|`). Use `&quot;` or switch to un-piped quoted
  labels `--> "text"`.
- **Special Characters in Labels**: Avoid unquoted labels containing
  parentheses `()`, brackets `[]`, or operators `< >` if they are not the
  primary node shape. Quote them or use HTML entities.
- **Special Chars in Pipe Labels**: Avoid structural characters (especially
  parentheses `()`) inside pipe-delimited labels `|...|`. Use quoted strings
  `--> "Label (text)"` instead.
- **Unquoted Parentheses & Strings**: **NEVER** use unquoted parentheses `()`
  or strings with spaces/commas/special chars without quotes in node labels,
  edge labels (`-->|...|`), or subgraph titles. Doing so triggers errors or
  parser skips. Always wrap them in exactly one pair of double quotes (e.g.,
  `subgraph ID ["Title (Details)"]`). Avoid parentheses entirely in edge
  labels (prefer `-->|Condition, details|` over `-->|Condition (details)|`).
