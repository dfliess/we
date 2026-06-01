# we plugin

Claude Code plugin for team engineering workflow. Three skills: `/we:grill` (domain modeling), `/we:setup` (project initialization), and `/we:onboard` (new-engineer onboarding).

## Structure

```
skills/
├── grill/          # Domain modeling sessions
│   ├── SKILL.md
│   ├── CONTEXT-FORMAT.md
│   └── ADR-FORMAT.md
├── setup/          # Project initialization
│   └── SKILL.md
└── onboard/        # New-engineer onboarding
    └── SKILL.md
```

## Rules

- Skills must be interactive (back-and-forth with user). If it can be a CLAUDE.md instruction, it shouldn't be a skill.
- Don't duplicate built-in Claude Code features (/code-review, /debug, /simplify).
- Keep skills under 100 lines. If a skill needs more, it's doing too much.
- Template content in skills should be minimal and adaptable, not rigid.
