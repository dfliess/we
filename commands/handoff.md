---
description: Compact the current conversation into a handoff document so a fresh agent (or a teammate) can pick the work up.
argument-hint: "What will the next session focus on?"
---

Write a handoff document summarising the current conversation so a fresh agent can continue the work.

## Where to save it

Decide with the user, don't assume:

- **Just continuing your own work in a new session?** Save to the OS temp directory (e.g. `$TMPDIR` on macOS, `/tmp` elsewhere). Ephemeral and safe for messy in-progress context.
- **Handing off to a teammate?** This is a `we` team workflow — offer to save it to `docs/handoffs/` in the repo instead, so it's shared. Confirm before writing into the repo.

If the user's intent is unclear from the conversation, ask which of the two it is before saving.

## What to write

- Tailor the document to what the next session will focus on. If arguments were passed (`$ARGUMENTS`), treat them as that focus and shape the doc around them.
- **Don't duplicate content already captured elsewhere** — PRDs, plans, ADRs, issues, commits, diffs, `CONTEXT.md`. Reference them by path or URL instead. (A handoff that restates the ADRs is bloat; one that points to them is useful.)
- Include a **"Suggested skills"** section naming the `we` skills the next agent should invoke — e.g. `/we:grill` if there are unresolved design decisions, `/we:onboard` if the next agent is unfamiliar with the area — plus any other relevant ones.
- Capture what a fresh agent can't reconstruct from the artifacts: current state, what was just tried, what's blocked, the next concrete step, and any decisions made verbally that haven't landed in an ADR yet.

## Safety

Redact anything sensitive — API keys, passwords, tokens, personally identifiable information. Never write secrets into the handoff.
