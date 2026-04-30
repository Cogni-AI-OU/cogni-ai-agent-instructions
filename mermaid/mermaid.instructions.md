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
- **Non-ASCII Characters**: Avoid typographic characters like `→` or `–` in
  labels. Using standard ASCII (`->`, `-`) ensures compatibility with
  ASCII-only linters (e.g., `require-ascii`) and improves cross-platform
  rendering consistency.
- **Over-complexity**: Avoid massive diagrams (> 50 nodes) that become
  unreadable.
- **Pipe-Delimited Label Restrictions**: **NEVER** use raw double quotes (`"`)
  or structural characters (especially parentheses `()`) inside pipe-delimited
  labels (e.g., `-->|...|`). Raw quotes crash many renderers; use `&quot;` or
  switch to un-piped quoted labels (e.g., `--> "Label (text)"`) to ensure
  stability.
- **Timeline Colon Delimiters**: **NEVER** use a colon (`:`) inside a time period 
  (e.g., `02:40`) or event description in a `timeline` diagram. The colon is strictly 
  reserved as the structural delimiter between periods and events. Use alternatives 
  like `02-40` or `02h40`.
- **Unquoted Strings & Special Chars**: **NEVER** use unquoted strings
  containing spaces, commas, parentheses `()`, brackets `[]`, curly braces `{}`,
  or operators `< >` in node labels, Kanban cards, or subgraph titles. Doing so
  triggers errors or parser skips. For Kanban cards (`[Label]`) or properties (`bodyText:`),
  completely strip or replace these characters. For other diagrams, always wrap them
  in exactly one pair of double quotes (e.g., `node_id["Label (Details)"]`).
