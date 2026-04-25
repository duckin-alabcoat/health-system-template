# ADR-006: Lane discipline — extract infrastructure state into system_status.md

**Date:** 2026-04-16
**Status:** Accepted

## Context

`state/health_state.md` was accumulating infrastructure content alongside health content. Aria, during her weekly synthesis, was referencing architect-level context — prompt versions, Prim run status, data pipeline connectivity, flag queue counts — and writing it into the health state file. This created several problems:

1. **Cross-lane reads:** Aria's synthesis step was effectively acting as an infrastructure reporter, a role that belongs to Prim
2. **Single Writer Per State violation (soft):** Aria was writing infrastructure observations that Prim was also monitoring — two agents holding overlapping views of the same infrastructure state
3. **Health state noise:** `health_state.md` is the first thing every specialist reads. Infrastructure metadata in that file wastes their token budget on context they don't need
4. **No Prim-owned infra record:** The architect had no single place to look for a current infrastructure snapshot. The health state file was the only place it lived, which meant reading it required reading all of the user's health data too

## Decision

Extract all infrastructure state from `health_state.md` into a new Prim-owned file: `state/system_status.md`.

**`state/health_state.md`:** Health only. Aria is the sole writer. Contains: current health priorities, active flags summary, agent domain summaries, open medical items. Never contains prompt versions, pipeline state, or architect decisions.

**`state/system_status.md`:** Infrastructure only. Prim is the sole writer. Contains: agent prompt versions reference (pointer to manifest), last Prim run timestamp and mode, data pipeline connectivity, flag queue counts, aria_today state, outbound queue state. Overwritten nightly on both modes. Aria reads it when she needs infra context during synthesis — she never mirrors its content back into health_state.md.

Version bumps: Aria v2.0 → v2.1 (Step 6 guardrail added), Prim v2.0 → v2.1 (new nightly function #6 writes the snapshot).

## Consequences

**Positive:**
- `health_state.md` is lean and health-focused — specialists get fast, relevant context on wake
- Infrastructure state has a canonical, Prim-owned home — the architect can check `system_status.md` without wading through health data
- Lane boundaries are clean: Aria synthesizes health, Prim monitors infrastructure, neither crosses into the other's domain
- Single Writer Per State is fully honored: health state → Aria, infra state → Prim

**Negative:**
- Interim window (April 16 → April 19): `health_state.md` v13 carries a ghost SYSTEM STATUS section that contradicts the new lane rule. Acceptable cost — Aria's next Sunday synthesis (April 19 03:00) will rewrite the file under v2.1 rules and drop the block naturally. The architect does not edit Aria's state file directly (Single Writer Per State).
- Prim's nightly run must now produce a coherent infrastructure snapshot even when some data sources are unavailable — failure to write `system_status.md` means Aria has no infra context on her next wake
