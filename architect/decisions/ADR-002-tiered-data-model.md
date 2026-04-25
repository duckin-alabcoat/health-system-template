# ADR-002: Tiered data model — hot / warm / cold

**Date:** 2026-04-06
**Status:** Accepted

## Context

The system has years of historical health data ([NUTRITION_APP] food logs going back to 2015, weight history since 2020, sleep data going back hundreds of days). Agents cannot read all of it on every wake — token budgets would be exhausted before any useful work happened.

At the same time, some data needs to be highly accessible (what did the user eat yesterday) while other data is rarely needed but must exist as a source of truth (what did the user weigh in 2022).

## Decision

Data is organized into three tiers:

**Hot (`data/hot/`) — last 4–8 weeks, detailed**
Agents read this for active work. Full granularity. Includes: [NUTRITION_APP] daily reports, Garmin training data, sleep data, CPAP records, race files. Prim manages the boundary — old hot data is moved to warm on the weekly run.

**Warm (`data/warm/`) — last 12 months, pre-summarized**
Summarized by week or month. Agents read this occasionally, when trend context is needed. Includes: weekly nutrition averages, monthly training summaries, medications list, supplements list, medical baselines (e.g., DEXA, lab results).

**Cold (`data/cold/`) — historical, insights only**
Raw data exists as a source of truth but agents never read it. Only summaries and insights are read from cold. Includes: weight history CSV (2020–March 2026), archived [NUTRITION_APP] logs (2015–Sept 2025), archived knowledge base entries, archived flags.

## Consequences

**Positive:**
- Agents stay fast and token-efficient — they read one small state file and then the hot tier they need
- Historical data is preserved without polluting active agent context
- Prim's archival function has clear, mechanical rules — move old hot entries to warm, move old warm entries to cold

**Negative:**
- Requires Prim to maintain tier boundaries reliably — a missed weekly run leaves hot data growing
- Summary fidelity depends on how well warm summaries were written; raw data in cold is the fallback but expensive to re-read
