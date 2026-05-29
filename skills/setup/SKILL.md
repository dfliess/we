---
name: setup
description: Initialize a new project with CLAUDE.md, CONTEXT.md, linter config, and pre-commit hooks. Use at the start of a new project to establish the team standard.
---

<what-to-do>

Set up this project with the team engineering standards. Do the following in order:

1. **Detect the project** — look at existing files to determine: language (Python, TypeScript, etc.), package manager, existing linter config, existing CLAUDE.md or CONTEXT.md.

2. **Ask what's needed** — based on what exists, ask the user which pieces to create. Don't overwrite existing files without asking. Typical pieces:
   - `CLAUDE.md` (AI instructions)
   - `CONTEXT.md` (domain glossary)
   - `docs/adr/` (architecture decisions directory)
   - Linter config (ruff for Python, eslint/biome for TypeScript)
   - `.pre-commit-config.yaml` (commit-time linting)

3. **Create the files** — use the templates below, adapted to what was detected.

4. **Verify** — run the linter to confirm the config works.

</what-to-do>

<supporting-info>

## CLAUDE.md template

Keep it under one screen. Structure:

```markdown
# {Project Name}

{One line: what this project is.} See [CONTEXT.md](./CONTEXT.md) for domain language.

## Commands

{3-5 commands: test, lint, run, dev}

## Code style

All code must pass `{linter}`. Rules are in `{config file}`. Write compliant code from the start.

## Domain

Read [CONTEXT.md](./CONTEXT.md) before writing code. Use the terms defined there.

## Architecture rules

{3-5 brief rules specific to this project's structure}

## Don't

{2-4 anti-patterns specific to this project}
```

Rules for writing CLAUDE.md:
- Reference linter config, don't restate rules
- Keep architecture rules to principles, not implementation details
- The "Don't" section captures lessons learned, not obvious things
- If it needs scroll, it's too long

## CONTEXT.md template

Use the format from [CONTEXT-FORMAT.md](../grill/CONTEXT-FORMAT.md). Start empty:

```markdown
# {Project Name}

{One sentence: what this project is and why it exists.}

## Language

<!-- Terms will be added during /we:grill sessions -->
```

## Linter config

### Python (ruff in pyproject.toml)

```toml
[tool.ruff]
line-length = 100
target-version = "py311"

[tool.ruff.lint]
select = ["E", "W", "F", "I", "UP", "B", "SIM", "RUF"]
```

### TypeScript (biome.json)

```json
{
  "$schema": "https://biomejs.dev/schemas/1.9.0/schema.json",
  "organizeImports": { "enabled": true },
  "linter": { "enabled": true, "rules": { "recommended": true } }
}
```

Adapt to the project's existing setup. Don't force a linter change if one is already configured.

## Pre-commit config

### Python

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.9.0
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format
```

### TypeScript

```yaml
repos:
  - repo: https://github.com/biomejs/pre-commit
    rev: v0.6.1
    hooks:
      - id: biome-check
        additional_dependencies: ["@biomejs/biome@1.9.0"]
```

## What NOT to create

- Don't create documentation files (README.md, CONTRIBUTING.md) — that's the project's job
- Don't create CI workflows — that's deployment-specific
- Don't create test config — that's framework-specific
- Don't add dependencies — only config files

</supporting-info>
