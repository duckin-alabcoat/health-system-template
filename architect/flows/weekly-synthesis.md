# Flow: Weekly Synthesis

The weekly synthesis is the system's primary memory consolidation cycle. It runs every week across Saturday and Sunday, producing the weekly report the user reviews Sunday evening.

---

## Timeline

| Time | Step |
|------|------|
| Saturday 3am | **Prim** weekly run — cleans and prepares |
| Saturday 4am | Agent batch — all 13 specialists file weekly inputs |
| Saturday 11pm | **Lyra** weight trend summary flag |
| Sunday 3am | **Aria** weekly synthesis — reads Saturday batch, writes `health_state.md` |
| Sunday 8pm | **Aria** weekly report delivery to the user |

---

## Saturday 3am — Prim Weekly Run

Prim runs her full weekly mode before the agent batch so the system is clean when agents write.

- Archives all closed flags from `state/flags.md` to `data/cold/flag-archive/`
- Trims `state/health_state.md` (moves old narratives to `reports/archived-state/`)
- Trims agent knowledge bases (moves old `THIS WEEK'S SESSIONS` entries to `data/cold/[agent]/` — intact, never summarized)
- Verifies agent prompt versions: compares `state/agent_manifest.md` against recent output stamps; flags mismatches for Aria
- Writes `state/system_status.md` snapshot (weekly mode)
- Writes `sessions/logs/prim/prim-weekly-YYYY-MM-DD.md`

---

## Saturday 4am — Agent Batch

All 13 specialists fire in the agent batch. Each agent:

1. Reads their outbound file if present
2. Reads their knowledge base
3. Reviews the week's data in their domain
4. Writes a summary flag to `state/flags.md` for Aria
5. Updates their knowledge base
6. Writes a session log

**Aria does not run in the Saturday batch.** She reads the batch outputs on Sunday at 3am.

---

## Saturday 11pm — Lyra Weight Summary

**Lyra** runs separately at 11pm (after the batch) to write a weight trend summary flag. Timing is deliberate — gives the day's weight data time to arrive from [NUTRITION_APP] before she summarizes.

---

## Sunday 3am — Aria Weekly Synthesis

Aria runs her full wake protocol against the Saturday batch:

1. Reads `state/aria_today.md`
2. Reads `state/outbound/aria.md` if present
3. Reads `## FOR ARIA` sections from all new session logs (the Saturday batch output)
4. Reads all open flags (the batch + any open flags from the week)
5. Synthesizes everything into `state/health_state.md` — updated, trimmed, current
6. Writes `state/aria_today.md` with anything useful for the Sunday 8pm wake

The Sunday 3am synthesis is the longest Aria run of the week — it is reading a full week's worth of specialist inputs.

---

## Sunday 8pm — Weekly Report Delivery

Aria wakes again and delivers the weekly report to the user. Format:

- Week summary — what happened, what didn't, what moved the needle
- Domain highlights from each specialist who had something noteworthy
- Training schedule for the week ahead (informed by Pace, Brynn, River, and the calendar)
- One or two decisions or choices for the user to weigh in on
- Open session proposals from any specialist requesting time this week

Aria writes the report to `reports/weekly/` with YAML frontmatter, then delivers it in the conversation.

---

## What Can Go Wrong

| Problem | Behavior |
|---------|----------|
| One specialist fails in Saturday batch | Other agents run normally; Prim will catch the missing session log on nightly Monday run |
| Prim weekly run fails Saturday 3am | Agent batch at 4am runs against unclean state; flags may not be archived before agents write new ones |
| Aria weekly synthesis fails Sunday 3am | health_state.md goes stale; Prim's nightly Monday run will flag the staleness for Aria at her 7am wake |
| [NUTRITION_APP] weekly email late | Flora's Monday 8:30am weekly task reconciles it when it arrives |
