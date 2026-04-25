# ADR-009: Prim as pure infrastructure — metadata in, metadata out

**Date:** 2026-04-08
**Status:** Accepted

## Context

As the system grew, a class of work emerged that wasn't health coaching: archiving old flags, trimming knowledge bases, verifying prompt versions, monitoring file staleness, counting sessions. Early designs had Aria doing some of this alongside her health coordinator work, which created two problems:

1. **Token waste:** Aria loading and processing all infrastructure state on every wake left less budget for actual health synthesis
2. **Role confusion:** When Aria reported "Prim ran clean" alongside "your sleep score was 72," it was unclear whether she was a health coordinator or a system monitor

## Decision

**Prim** is the 13th agent and owns all infrastructure work. Her mandate: everything the architect would manually monitor eventually falls to Prim.

**Prim's constraint:** Metadata in, metadata out. She reads file metadata (timestamps, frontmatter fields, session counts, flag statuses) and writes infrastructure summaries (`system_status.md`, nightly/weekly logs). She never reads the *content* of session notes or knowledge bases — she only reads their metadata. She never summarizes, interprets, or edits any agent's health-domain content.

This means Prim can trim a knowledge base by date without reading what's in the entries she's moving. She moves by date field in frontmatter, not by content.

**Prim runs in two modes, both at 3am:**
- **Nightly (Sun–Fri):** Staleness checks, session counting, aria_flag processing, outbound monitoring, aria_today monitoring, system_status.md snapshot
- **Weekly (Saturday):** Everything in nightly, plus flag archival, health_state.md trim, knowledge base trim, agent version verification

3am timing is deliberate — the user sometimes codes until 2am; 3am is effectively guaranteed clear.

## Consequences

**Positive:**
- Aria's wake protocol stays health-focused — she reads health state and health flags, not infrastructure reports
- Infrastructure monitoring is automatic and consistent — Prim runs every day regardless of whether the user interacts with the system
- Clear accountability: if an infrastructure problem is missed, the answer is always "Prim should have caught it"

**Negative:**
- Prim's "metadata only" constraint means she cannot make intelligent decisions about content — she can detect that a session log is old, but cannot evaluate whether its content is still relevant
- Prim's 3am scheduled runs cannot be triggered by events (e.g., she cannot run immediately when a new session log is written). Everything waits for the next scheduled wake. Iris (open action item #23) will eventually address this for ad-hoc infrastructure tasks.
