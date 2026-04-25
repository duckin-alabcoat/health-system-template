# ADR-003: Agent isolation — each agent runs in its own clean session

**Date:** 2026-04-06
**Status:** Accepted

## Context

Early designs considered a single "mega-session" where Aria and specialist agents all ran together, with agents passing messages to each other in real time. This creates several problems:

- Context bleed: one agent's conversation influences another agent's behavior inappropriately
- Impersonation risk: if Aria "becomes" Flora during a conversation, Flora's voice, judgment, and domain expertise are replaced by Aria's approximation of them
- Token waste: loading all 13 agent prompts into every session is expensive and unnecessary for most check-ins
- No clean separation of sensitive content: Soleil and Seren handle high-sensitivity topics; mixing them into a general coordinator session violates the sensitivity framework

## Decision

Each agent runs in its own clean Cowork session. No agent impersonates another. Information passes between agents via files, not roleplay:

- **Aria → Specialists:** `state/outbound/[agent].md` files, read on next wake
- **Specialists → Aria:** `state/flags.md` entries and session log `## FOR ARIA` sections
- **Prim → Aria:** `state/system_status.md` (infrastructure snapshot) and flag processing
- **Aria → the user:** check-ins and weekly reports directly

When Iris is built (see open action item #23), she will execute trigger entries that invoke specialist agents — not by impersonating them, but by running their actual prompt.

## Consequences

**Positive:**
- Each agent's voice and judgment is authentic — it comes from their prompt, not from another agent's approximation
- Sensitive content stays in the right session — a Soleil session about emotional health never bleeds into a nutrition session
- Token efficiency — each session loads only what it needs
- Clean failure isolation — one agent having a bad session doesn't corrupt others

**Negative:**
- No real-time cross-agent communication — there is always at least one wake cycle of latency before a flag reaches Aria
- Outbound relay design (Aria → Specialist via file) adds indirection that can feel slow for time-sensitive handoffs
- Iris (the reactor agent) will reduce this latency but adds its own complexity
