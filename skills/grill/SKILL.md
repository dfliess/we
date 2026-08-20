---
name: grill
description: Grilling session that challenges your plan against the existing domain model, sharpens terminology, and updates documentation (CONTEXT.md, ADRs) inline as decisions crystallise. Asks in rounds, as plain chat questions with a recommended answer each. Use when user wants to stress-test a plan against their project's language and documented decisions.
---

<what-to-do>

Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Map it as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask now without guessing at answers you haven't heard yet. Ask the whole frontier in one round, then stop and wait for my answers before the next round. A question whose answer depends on another question still open in this round belongs to a later round, not this one.

My answers reshape the tree: settled decisions push the frontier outward and unblock the questions that depended on them. Recompute the frontier and ask the next round.

## How to ask

Write each round into the chat as markdown, one block per question:

```
❓ **Q1** - **<question title>**: <question body, may run several paragraphs, may lay out options>

➡️ <your recommended answer>
```

Never ask through a structured question tool (in Claude Code, `AskUserQuestion`). Its option chips cap the answer space at the options you thought of and hide the reasoning behind each one. A grilling session needs prose answers, "none of those", and counter-questions. Plain markdown in the chat, always.

## Facts are yours, decisions are mine

Finding facts is your job, never mine. When a frontier question needs a fact from the environment (the codebase, the filesystem, a tool), go and find it, or dispatch a subagent to find it. Don't ask me anything you could look up. Don't block on it either: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the answer. Ask the rest of the frontier now.

The decisions are mine. Put each one to me and wait. Answering your own questions and carrying on breaks the session.

## When I grill you back

I may answer a question with a question. That is not a derail, it is me thinking: sometimes the fastest way to settle a decision is to grill you back. Answer it straight and in full, with the trade-offs and your own opinion, and go read the codebase if the answer is there. Don't deflect it back at me, and don't count it as my answer to the question you asked.

Then pick the round back up: restate the questions I left open rather than dropping them.

Keep it tethered. The discussion serves the decision on the table. If it opens a new branch of the design tree, name it and park it as a question for a later round instead of following it now.

## When it's done

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on the plan until I confirm we have reached a shared understanding.

</what-to-do>

<supporting-info>

## Domain awareness

During codebase exploration, also look for existing documentation:

### File structure

Most repos have a single `CONTEXT.md` at the root, with ADRs in `docs/adr/` (`0001-event-sourced-orders.md`, `0002-postgres-for-write-model.md`).

If a `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts: each `CONTEXT.md` lives beside the code it describes (`src/ordering/CONTEXT.md`) with its own `docs/adr/`, and the root `docs/adr/` holds system-wide decisions. The map points to where each context lives.

Create files lazily: only when you have something to write. If no `CONTEXT.md` exists, create one when the first term is resolved. If no `docs/adr/` exists, create it when the first ADR is needed.

## During the session

### Challenge against the glossary

When the user uses a term that conflicts with the existing language in `CONTEXT.md`, call it out immediately. "Your glossary defines 'cancellation' as X, but you seem to mean Y. Which is it?"

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account': do you mean the Customer or the User? Those are different things."

### Discuss concrete scenarios

When domain relationships are being discussed, stress-test them with specific scenarios. Invent scenarios that probe edge cases and force the user to be precise about the boundaries between concepts.

### Cross-reference with code

When the user states how something works, check whether the code agrees. If you find a contradiction, surface it: "Your code cancels entire Orders, but you just said partial cancellation is possible. Which is right?"

### Update CONTEXT.md inline

When a term is resolved, update `CONTEXT.md` right there. Don't batch these up: capture them as they happen. Use the format in [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md).

`CONTEXT.md` should be totally devoid of implementation details. Do not treat `CONTEXT.md` as a spec, a scratch pad, or a repository for implementation decisions. It is a glossary and nothing else.

### Offer ADRs sparingly

Only offer to create an ADR when all three are true:

1. **Hard to reverse**: the cost of changing your mind later is meaningful
2. **Surprising without context**: a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off**: there were genuine alternatives and you picked one for specific reasons

If any of the three is missing, skip the ADR. Use the format in [ADR-FORMAT.md](./ADR-FORMAT.md).

</supporting-info>
