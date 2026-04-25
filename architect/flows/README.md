# Flows

Key data and workflow flows in the health system. Each document describes a flow end-to-end: trigger, steps, outputs, and failure modes.

## Index

| Flow | File | Summary |
|------|------|---------|
| Daily Nutrition Pipeline | [daily-nutrition-pipeline.md](daily-nutrition-pipeline.md) | Flora's 8:15am run — parses [NUTRITION_APP] email, files data, suspicious-day scan, flags Aria |
| Weekly Synthesis | [weekly-synthesis.md](weekly-synthesis.md) | Saturday Prim clean → Saturday agent batch → Sunday Aria synthesis → Sunday report |
| Manual Entry | [manual-entry.md](manual-entry.md) | Interactive Flora session to correct or complete a past day's nutrition data |

## Flows Not Yet Documented

- **Prim nightly run** — staleness check, session counting, flag processing, outbound monitoring
- **Aria morning/evening check-in** — wake protocol, distraction layer, synthesis, delivery
- **Luna evening touchpoint** — handoff read, sleep coaching delivery
- **Post-appointment debrief** — Vera debrief → Aria update → system sync
- **Suspicious-day resolution** (via Aria relay) — described in manual-entry.md but could stand alone
