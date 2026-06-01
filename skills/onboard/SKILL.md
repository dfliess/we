---
name: onboard
description: Interactive onboarding session that brings a new engineer up to speed on this codebase — its domain language, architecture decisions, and structure — tailored to what they already know and what they're about to work on. Use when someone joins a project or starts in an unfamiliar area of it.
---

<what-to-do>

Bring me up to speed on this codebase through a guided, interactive session — not a data dump. The live conversation is the deliverable; do not generate an onboarding document.

1. **Gauge me first** — ask, one question at a time, waiting for each answer: What's my background? What am I here to build or fix? How familiar am I already with this domain and stack? Tailor everything that follows to my answers.

2. **Build the mental model from what's documented** — read the project's own materials before explaining anything: `CLAUDE.md`, `CONTEXT.md` (or `CONTEXT-MAP.md` if multiple contexts), and `docs/adr/`. Teach from these, don't restate them verbatim.

3. **Walk the path I'll actually work on** — trace the code relevant to my task: entry points, the main seams, where my area connects to the rest. Show me real files (`file_path:line`), don't describe in the abstract.

4. **Teach the language** — the shared glossary is the point. Surface the `CONTEXT.md` terms I'll need and the synonyms to avoid. If I use a term loosely, correct it the way the glossary defines it.

Ask before each deep-dive whether I want to go deeper or move on. Stop when I have what I need for my task — don't tour the whole repo.

</what-to-do>

<supporting-info>

## Domain awareness

Find the documentation the same way `/we:grill` does. Most repos have a single context:

```
/
├── CLAUDE.md
├── CONTEXT.md
└── docs/adr/
    ├── 0001-event-sourced-orders.md
    └── 0002-postgres-for-write-model.md
```

If a `CONTEXT-MAP.md` exists at the root, the repo has multiple bounded contexts — each with its own `CONTEXT.md` and `docs/adr/`. Orient me to the one my task lives in first; mention the others exist but don't tour them.

## Lean on ADRs for the "why"

When you walk code that looks surprising, check `docs/adr/` for the decision behind it. "This uses event sourcing — ADR 0001 explains why, the trade-off was X." A newcomer's biggest gap is *why*, not *what*; the ADRs are where the why lives.

## Surface gaps — don't paper over them

If you hit something the docs should explain but don't, say so plainly rather than inventing an explanation:

- A term I'll need that isn't in `CONTEXT.md` → flag it, and offer to run `/we:grill` to pin it down.
- Code that contradicts what `CONTEXT.md` or an ADR says → surface the contradiction; it's a real finding, not my confusion.
- A decision that's load-bearing but has no ADR → note it; it may be worth recording.

Onboarding is also a documentation audit: a newcomer's questions are the best signal of what the docs are missing.

## What NOT to do

- Don't generate an onboarding doc, checklist file, or summary file — the session is the deliverable.
- Don't restate `CLAUDE.md`/`CONTEXT.md` back to me — teach from them, then point me at them.
- Don't tour the whole codebase — follow the thread of my task.
- Don't explain things I already told you I know.

</supporting-info>
