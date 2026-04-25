# ADR-007: Flora manual entry — design decisions

**Date:** 2026-04-16
**Status:** Accepted

## Context

Flora's automated pipeline (daily 8:15am) assumes the [NUTRITION_APP] email carries authoritative data for the target date. This assumption breaks in two ways:

1. the user doesn't log on day D and updates [NUTRITION_APP] on D+1 or D+2. The daily email for D was already sent with incomplete or zero data. Flora's 14-day backfill scan re-reads the same stale email — the late edits never arrive by email.
2. The [NUTRITION_APP] email marks a day as "nothing logged" even when the user did eat — he just didn't open the app. There was no mechanism for him to hand Flora the real numbers.

The system needed a way for the user to provide the real data for a past date and have Flora update the file with a full audit trail.

## Decision

### Input shape: free-form in, structured readback out
the user provides data in any format — conversation ("I had salmon and a protein shake"), [NUTRITION_APP] screenshot (Flora reads the image), description/photo (Flora estimates). All nutrition data is estimate-stacked at every layer: the user estimates portions, [NUTRITION_APP] estimates nutrients. A manual handover is just another estimate of the same shape. Flora does not chase false precision. Target: reasonable estimate of what the user ate.

### Readback before writing — no silent writes
Flora never writes directly from the user's input. She parses the data, proposes a diff (`[new] (was [old])` per changed field, explicit "unchanged" for others), and writes only after the user confirms. This prevents misparses from silently corrupting the data file.

### Merge semantics
Partial corrections merge — Flora writes the fields the user provided, leaves the rest at their current values. The whole-file `processed_by` flips to `Flora-manual-entry`. No per-field provenance tracking. Readback shows changed vs unchanged explicitly.

### Source hierarchy
Manual entries always win over [NUTRITION_APP]. If the weekly reconciliation later finds a conflict between a manual-entry file and the [NUTRITION_APP] weekly report (>50 cal or >5g protein), Flora keeps the manual number and flags the conflict to Aria. Aria decides whether to surface it to the user.

### Suspicious-day detection
Five hard triggers that Flora checks in the 14-day window on every daily run: (1) [NUTRITION_APP] email literally said "nothing logged," (2) any main meal slot empty while others have entries, (3) total calories <800 on a non-fasting day, (4) protein <50g with total calories >1,000, (5) weight recorded with zero food logged. Plus a judgment layer for borderline cases. Flags are concrete and decision-ready — not stubs. They stay open until the user corrects or confirms.

### Surface path
Two routes from a suspicious-day flag:
- **Complex (actual numbers):** Interactive Flora session — free-form input → readback → confirm → write
- **Lightweight (confirm as-is):** the user tells Aria at check-in the day was accurate. Aria writes `state/outbound/flora.md`. Flora stamps the data file `user_confirmed_as_is: true` on her next scheduled wake. No Flora session needed.

### Aria's relay role
Aria relays simple confirmations only. She does not transcribe nutrition data. Any real correction routes to an interactive Flora session. This keeps lane discipline intact — Aria is a scheduler and coordinator, not a nutrition data entry agent.

### No dedicated handover index
Decision: no index file tracking manual entries over time. The data file frontmatter is the audit record. If pattern-scanning becomes useful later, adding an index is cheap; removing one is annoying.

## Consequences

**Positive:**
- Breaks the dependency on [NUTRITION_APP] email completeness — the user can always get the right data into the system
- Readback-before-write prevents silent data corruption from misparses
- Suspicious-day detection surfaces gaps automatically rather than waiting for the user to notice
- The lightweight confirm-as-is path via Aria relay keeps simple interactions simple

**Negative:**
- Flora's daily workflow is now more complex — suspicious-day scan adds a step that requires checking flags.md to avoid duplicates
- The Aria relay adds a new outbound file format and behavioral rule to both Flora and Aria prompts
- Manual entries that differ significantly from [NUTRITION_APP] weekly reports will generate noise flags to Aria — in practice this should be rare but will happen occasionally
