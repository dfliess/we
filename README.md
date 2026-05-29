# we

A [Claude Code](https://claude.ai/code) plugin for teams that want shared engineering practices without the bloat.

## Philosophy

Most engineering problems aren't tooling problems — they're alignment problems. Teams ship slow not because they lack automation, but because they don't share a language, don't document decisions, and repeat the same debates.

`we` fixes the alignment layer:

1. **Shared domain language** — every project gets a glossary (CONTEXT.md) so the team and the AI use the same terms
2. **Decisions on record** — architecture decisions are captured as ADRs when they're hard to reverse, surprising without context, and the result of a real trade-off
3. **Project setup that sticks** — new projects start with the right structure from day one

Everything else — code review, testing, deployment — Claude Code already does well with a good CLAUDE.md. `we` doesn't duplicate built-in features.

## Skills

| Command | What it does |
|---|---|
| `/we:grill` | Domain modeling session. Challenges your plan, sharpens terminology, creates/updates CONTEXT.md and ADRs as decisions crystallise. |
| `/we:setup` | Initializes a new project with CLAUDE.md, CONTEXT.md, ruff config, and pre-commit hooks. One-time setup that establishes the team standard. |

## Install

In Claude Code:

```
/plugin marketplace add dfliess/we
/plugin install we
```

## Workflow

### Starting a new project

```
/we:setup
```

This creates:
- `CLAUDE.md` — instructions for the AI (commands, code style reference, architecture rules)
- `CONTEXT.md` — empty domain glossary, ready for terms
- `docs/adr/` — directory for architecture decisions
- Ruff lint config in `pyproject.toml` (Python) or equivalent
- `.pre-commit-config.yaml` for commit-time linting

### Before building a feature

```
/we:grill we need to add user authentication
```

The grill session will:
- Ask you questions one at a time about your design
- Challenge vague terms ("you said 'account' — do you mean Customer or User?")
- Cross-reference with existing code to catch contradictions
- Update CONTEXT.md as terms are resolved
- Offer ADRs only when the decision is genuinely worth recording

### Day-to-day coding

No special skills needed. A well-written CLAUDE.md is enough:
- Code style → reference linter config, don't restate rules
- Architecture patterns → brief rules in CLAUDE.md, not a manual
- Domain language → AI reads CONTEXT.md automatically
- Code review → use built-in `/code-review`
- Debugging → use built-in `/debug`

### What goes where

| What | Where | Why |
|---|---|---|
| Domain terms | `CONTEXT.md` | Shared language, not implementation |
| Architecture decisions | `docs/adr/` | Record the *why*, not the *what* |
| Code style rules | Linter config | Tool-enforced, not document-enforced |
| AI instructions | `CLAUDE.md` | Brief, references other files |
| Project docs | `README.md` | For humans, not for AI |

### What NOT to do

- Don't put implementation details in CONTEXT.md — it's a glossary
- Don't restate linter rules in CLAUDE.md — reference the config
- Don't create a skill for something you do less than 3x per week
- Don't create ADRs for obvious decisions — only for surprising ones
- Don't write a 50-page CLAUDE.md — if it needs scroll, it's too long

## Why so few skills?

Most "AI workflow" tools fail by adding complexity. A team with 20 custom skills uses 3 of them.

The rule: if CLAUDE.md instructions can do it, skip the skill. Skills exist for interactive workflows that need back-and-forth (like domain modeling), not for things the AI can follow from a one-liner.

Built-in Claude Code features already cover: code review (`/code-review`), debugging (`/debug`), simplification (`/simplify`), and batch operations (`/batch`). We don't duplicate them.
