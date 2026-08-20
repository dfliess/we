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
| `/we:onboard` | Interactive onboarding session. Brings a new engineer up to speed on the domain language, ADRs, and code — tailored to their task. Surfaces doc gaps as it goes. |

## Commands

Skills are interactive; commands are one-shot. When a job has no back-and-forth, it's a command, not a skill.

| Command | What it does |
|---|---|
| `/we:handoff` | Compacts the current conversation into a handoff doc so a fresh agent — or a teammate — can pick the work up. References existing artifacts by path instead of restating them; redacts secrets. |

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
- Ask in rounds — every question whose prerequisites are settled, each with a recommended answer, as plain chat text rather than through the option-chip question tool
- Answer *your* questions back when you'd rather discuss a decision than settle it, then pick the round up where it left off
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

### Tracking work — issues vs agent tasks

Two levels, kept separate (the spec-driven pattern: a spec/issue is the *what*; the agent's task list is the *how*):

| Level | What it is | Where it lives | Persistent? |
|---|---|---|---|
| **Issue / user story** | what + why + acceptance criteria | GitHub Issues + kanban | yes |
| **Agent plan + tasks** | steps to fulfil one issue | ephemeral: plan mode + the harness task tools | no (per session) |
| **Handoff** | when an issue spans sessions | `/we:handoff` (references the issue) | one-shot |

The rule that keeps them apart: *would you show it to a PM on the board? → issue. Is it a step to fulfil an issue? → an agent task that traces to the issue, not a board item.*

Flow: the roadmap/epics break down into issues → each issue gets its own branch → a PR closes it. Agents create and update issues via the GitHub MCP (or `gh`) and never commit to a shared branch; execution steps stay in plan mode, off the board. Adopt the mental model, not a tool — don't add Spec-Kit/Kiro, that's bloat.

Issue shape — every issue reads like a spec: **background, goal, acceptance criteria, constraints**. Materialize it as a GitHub issue template (`.github/ISSUE_TEMPLATE/`) so the shape is enforced, not remembered.

### Branch naming — Conventional Branches

Follow [Conventional Branches](https://conventionalbranch.org/) for all new branches. Format: `<type>/<issue>-<kebab-description>`.

| Type | Use |
|---|---|
| `feature` | New features |
| `bugfix` | Bug fixes |
| `hotfix` | Urgent production fixes |
| `release` | Release preparation |
| `chore` | Maintenance, docs, config |

Trunk branch: `main`. Lowercase, hyphens only, no consecutive/trailing hyphens. Include the issue number when one exists (e.g., `feature/42-user-auth`, `bugfix/15-null-check`).

### What goes where

| What | Where | Why |
|---|---|---|
| Domain terms | `CONTEXT.md` | Shared language, not implementation |
| Architecture decisions | `docs/adr/` | Record the *why*, not the *what* |
| Code style rules | Linter config | Tool-enforced, not document-enforced |
| AI instructions | `CLAUDE.md` | Brief, references other files |
| Project docs | `README.md` | For humans, not for AI |
| Work items (issues/user stories) | GitHub Issues + kanban | Mutable state; rots in the repo |

### What NOT to do

- Don't put implementation details in CONTEXT.md — it's a glossary
- Don't restate linter rules in CLAUDE.md — reference the config
- Don't create a skill for something you do less than 3x per week
- Don't create ADRs for obvious decisions — only for surprising ones
- Don't write a 50-page CLAUDE.md — if it needs scroll, it's too long
- Don't track work as TODO.md/checklists in the repo — use GitHub Issues
- Don't put agent execution steps on the board — they trace to an issue, they aren't issues
- Don't mix languages in repo artifacts — code, commits, issues, ADRs, CONTEXT, docs in English; chat in your team's language

## Why so few skills?

Most "AI workflow" tools fail by adding complexity. A team with 20 custom skills uses 3 of them.

The rule: if CLAUDE.md instructions can do it, skip the skill. Skills exist for interactive workflows that need back-and-forth (like domain modeling), not for things the AI can follow from a one-liner.

Built-in Claude Code features already cover: code review (`/code-review`), debugging (`/debug`), simplification (`/simplify`), and batch operations (`/batch`). We don't duplicate them.

## Marketplace — under investigation

Third-party tools we're evaluating to curate into this marketplace. **Not endorsed or bundled yet** — pointers only, pending a decision on whether to wrap them as `we` plugins or leave them as recommendations. Of these, only Open Code Review is already a Claude Code plugin (installable directly); Taste is a SKILL and Context7 is an MCP server, so those two would need a thin wrapper to be installable here.

| Candidate | Fills | Status | Notes |
|---|---|---|---|
| [Taste](https://www.tasteskill.dev/) ([repo](https://github.com/Leonxlnx/taste-skill)) | Frontend design quality (anti-slop layout/typography/motion) | 🔍 To investigate | SKILL.md, not a plugin. Richer than the official `frontend-design`. Star candidate since we do real UI. Install today: `npx skills add https://github.com/Leonxlnx/taste-skill` |
| [Context7](https://context7.com/) | Up-to-date library docs in context (fewer API hallucinations) | 🔍 To investigate | MCP server, not a plugin. Universally useful, doesn't duplicate built-ins. MCPs change fast — leaning toward keeping this a pointer, not a wrapper. |
| [Open Code Review](https://github.com/alibaba/open-code-review) | Code review on **large** changesets — deterministic file-bundling + line-level precision | 🔍 To investigate | Already a Claude Code plugin (`/open-code-review:review`), multi-LLM, battle-tested at Alibaba, CI-ready. Niche vs built-in `/code-review`: big PRs + position accuracy. Curate only if the built-in falls short there. |

Deliberately **not** added (already built-in or official, would be bloat): `code-review`, `frontend-design`, `security-guidance`, `feature-dev`, `pr-review-toolkit`.
