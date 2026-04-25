# ADR-004: Single Writer Per State

**Date:** 2026-04-09
**Status:** Accepted

## Context

Multiple agents read and write to shared files. Without clear ownership, two agents could write to the same state file in the same session window, producing corrupted or contradictory state. iCloud Drive has no locking primitives — there is no way to acquire a write lock before writing.

The problem is most acute for `state/health_state.md`, which is the first thing every specialist reads on wake. If Aria and Prim both wrote to it, or if a specialist wrote their own data directly into it, the file would become incoherent.

## Decision

Every shared state file has exactly one writer. No other agent writes to it under any circumstances.

| File | Sole Writer | Readers |
|------|-------------|---------|
| `state/health_state.md` | **Aria** | All specialists (on wake) |
| `state/system_status.md` | **Prim** | **Aria** (when infra context needed) |
| `state/flags.md` | Specialists append; **Prim** archives | **Aria** (processes); **Prim** (archives) |
| `state/outbound/[agent].md` | **Aria** | Named specialist (reads + deletes) |
| `data/hot/nutrition/` | **Flora** | **Aria**, **Lyra** |
| `sessions/logs/[agent]/` | Named agent only | **Aria** (FOR ARIA section only) |
| `state/agent_manifest.md` | Architect (humans only) | **Prim** (version verification) |

## Consequences

**Positive:**
- File corruption from concurrent writes is eliminated by design, not by locking
- Easy to audit: if `health_state.md` is wrong, the answer is always "Aria wrote something incorrect" — there is no question of which agent is responsible
- Clear mental model for the architect when debugging

**Negative:**
- Every cross-agent data flow must go through an intermediary (flags, outbound files) rather than direct writes
- Aria becomes a bottleneck — all health state synthesis flows through her, so a bad Aria session affects the whole system
- The rule requires discipline in prompt writing — every new agent prompt must be explicit about what it may and may not write
