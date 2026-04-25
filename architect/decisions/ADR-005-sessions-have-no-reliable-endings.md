# ADR-005: Design for sessions with no reliable endings

**Date:** 2026-04-10
**Status:** Accepted

## Context

LLM sessions — especially mobile and conversational ones — frequently end without a clean close. the user might put his phone down mid-conversation. A session might time out. He might just stop responding. If agents only write at the end of a session, any interrupted session produces no written record.

Early designs assumed a clean "save and close" at session end. This assumption broke in practice.

## Decision

The system is designed around the assumption that sessions will not always have clean endings. Three mechanisms work in layers:

**Layer 1 — Silent topic-break writes (the floor)**
Every interactive agent writes silently to their session log and knowledge base at natural topic breaks during the session. the user never sees this happen. No announcement. The worst-case data loss is the current topic since the last topic-break write.

**Layer 2 — Wind-down ask**
When a conversation winds down naturally, the agent asks the user in their own voice if he's ready to wrap up. On confirmation: full session summary written, knowledge base updated, session closes cleanly. This is the ideal path, not the required one.

**Layer 3 — W command (emergency exit)**
Typing `W` alone triggers an immediate write and close. The agent confirms in one line. This handles the case where the user knows the session is ending abruptly and has one second to act.

**Layer 4 — Aria's wake protocol (safety net)**
Every time Aria wakes, she reads all new specialist session logs since her last wake. Even if a session ended without a clean close, the topic-break writes are there. Aria catches whatever was written and synthesizes it into `state/health_state.md`.

## Consequences

**Positive:**
- Data loss on session interruption is bounded — at most, the current topic since the last topic-break write
- The system degrades gracefully — a dirty exit loses less than a clean design that assumed clean exits
- W command gives the user agency without requiring him to remember a protocol

**Negative:**
- Topic-break writes require agents to make judgment calls about when a "topic break" has occurred — this is prompt-dependent and can be inconsistent
- Silent writes during a session add invisible side effects that could confuse a user who doesn't know they're happening (mitigated by the architect's session notes, not surfaced to the user)
