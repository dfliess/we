# we

Plugin de Claude Code que define cómo trabaja un equipo de ingeniería: lenguaje de dominio compartido, decisiones documentadas, y setup de proyecto consistente.

## Language

**Skill**:
An interactive workflow invoked with `/we:<name>`. Skills exist for back-and-forth sessions that can't be reduced to a CLAUDE.md instruction. If it can be a one-liner, it shouldn't be a skill.
_Avoid_: Command, tool, script

**Grill Session**:
An interactive domain modeling session (`/we:grill`) where the AI challenges the user's design, sharpens terminology, and updates CONTEXT.md and ADRs inline as decisions crystallise.
_Avoid_: Interview, questionnaire, review

**CONTEXT.md**:
A domain glossary that defines canonical terms and their avoided synonyms. Devoid of implementation details. One per bounded context.
_Avoid_: Glossary file, domain doc, spec

**ADR**:
An Architecture Decision Record. Created only when a decision is hard to reverse, surprising without context, and the result of a real trade-off. Lives in `docs/adr/`.
_Avoid_: Design doc, RFC, spec
