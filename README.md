# we

Team engineering workflow plugin for [Claude Code](https://claude.ai/code).

## Skills

| Skill | Command | Description |
|---|---|---|
| **grill** | `/we:grill` | Domain modeling session — challenges your plan, sharpens terminology, updates CONTEXT.md and ADRs inline |

## Install

```bash
/plugin install we@dfliess/we
```

## What it produces

- **CONTEXT.md** — Domain glossary with canonical terms and avoided synonyms
- **docs/adr/** — Architecture Decision Records (only when genuinely needed)
- **CONTEXT-MAP.md** — Multi-context repos only
