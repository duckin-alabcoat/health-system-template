# ARCHITECTURE

A personal health management system built on top of Claude (Cowork / Claude Agent SDK). 13 specialist agents plus a coordinator, all running as independent Cowork sessions, coordinated through shared files in iCloud Drive (or any shared folder).

---

## Overview

The system replaces the ad-hoc experience of asking an AI about your health with a persistent, multi-agent network that remembers context across sessions, monitors data automatically, and surfaces the right information at the right time. The user interacts almost exclusively with **Aria**, the coordinator. The specialist agents work in the background and route findings through her.

The system is personal infrastructure, not a product. It is designed to be customized around one person's health profile, devices, and life constraints.

---

## Design Principles

**Research First** — agents gather data, history, and context before asking the user anything. They never ask what they could find themselves.

**Agent Isolation** — each agent runs in its own clean Cowork session. No agent impersonates another. Information passes between agents via files, not roleplay. See ADR-003.

**Single Writer Per State** — every shared file has exactly one agent authorized to write it. No exceptions. See ADR-004.

**Lane Discipline** — `state/health_state.md` is health only (Aria's domain). `state/system_status.md` is infrastructure only (Prim's domain). Agents write only to files in their lane.

**Sessions Have No Reliable Endings** — agents write at topic breaks during sessions, not just at the end. The system degrades gracefully on interrupted sessions. See ADR-005.

**Prim Is Operations** — everything the architect would manually monitor eventually falls to Prim. Metadata in, metadata out. Content is never her business. See ADR-009.

**Goal-Oriented, Not Streak-Obsessed** — the system tracks progress toward goals. Missing days is data, not failure.

**Token Efficiency** — agents read the minimum context needed. State file first, then hot-tier data. They never read years of raw history.

**Medical Boundary** — agents prepare and surface. Doctors decide.

---

## The Agents

| # | Name | Role | Schedule |
|---|------|------|----------|
| 1 | **Aria** | Coordinator — user's only daily interface | Daily 7am, 7pm; Sunday 3am synthesis, 8pm report |
| 2 | **Pace** | Cardio coach | Saturday 4am weekly input; reactive |
| 3 | **Brynn** | Strength coach | Saturday 4am weekly input; reactive |
| 4 | **Flora** | Nutrition | Daily 8:15am; Monday 8:30am weekly; reactive |
| 5 | **Luna** | Sleep coach | Daily 9:30pm |
| 6 | **Vera** | Doctor's assistant | Triggered at T-3, T-1, post-appointment |
| 7 | **Soleil** | Mental health | Wednesday 3:30am weekly scan |
| 8 | **Lumi** | Personal care | Saturday 4am weekly input; reactive |
| 9 | **Seren** | Sexual health | Saturday 4am weekly input; reactive |
| 10 | **Vita** | Longevity research | 1st and 15th at 5am biweekly research; reactive |
| 11 | **River** | Hydration, recovery & cross-training load | Saturday 4am weekly input; reactive |
| 12 | **Lyra** | Weight & body composition | Saturday 4am weekly input; Saturday 11pm summary |
| 13 | **Prim** | System operations — infrastructure only | Daily 3am (nightly); Saturday 3am (weekly) |
| 14 | **Iris** | Reactor — trigger queue executor | Daily 9am, 1pm, 6pm |

**Scope notes:**
- **Pace** handles cardio/endurance training only. Strength and cross-training are out of scope.
- **Brynn** handles strength training only. Cross-training activities are out of scope.
- **River** handles all non-cardio, non-lifting activity as training load — yoga, Pilates, cycling, swimming, group fitness — plus hydration and recovery. River accounts for these activities; she does not coach them.
- **Aria** is the training scheduler. She sees all three (Pace, Brynn, River) outputs and makes cross-week scheduling decisions. She does not coach individual activities.

---

## Communication Architecture

Agents do not talk to each other in real time. All communication is asynchronous, file-based, and routed through Aria.

### Specialist → Aria

**Flags (`state/flags.md`):** The primary channel. Every specialist appends YAML-frontmatter flag blocks when they have something for Aria. Aria reads all open flags on every wake and marks acted flags closed. Prim archives closed flags on her Saturday weekly run.

```
---
agent: Flora
timestamp: YYYY-MM-DD HH:MM
priority: routine | attention | urgent
action: FYI | schedule session | surface to user now
status: open
note: [message]
---
```

**Session logs (`sessions/logs/[agent]/`):** After every meaningful interactive session, agents write a structured log. Aria reads only the `## FOR ARIA` section (5–6 lines max) of new logs on each wake.

### Aria → Specialists

**Outbound files (`state/outbound/[agent].md`):** When Aria has something for a specialist, she writes a file to the outbound folder. The file only exists when there is a pending message. The specialist reads and deletes it on their next wake.

### Infrastructure → Aria

**`state/system_status.md`:** Prim writes this snapshot nightly. Aria reads it when she needs infrastructure context. It covers: prompt versions, last Prim run, data pipeline state, flag queue counts, outbound queue state.

### Routing Rule

All cross-agent communication routes through Aria. Specialists do not message each other directly.

---

## State Files

| File | Owner | Purpose |
|------|-------|---------|
| `state/health_state.md` | Aria | Live health snapshot. Health only. |
| `state/system_status.md` | Prim | Infrastructure snapshot. Overwritten nightly. |
| `state/flags.md` | Agents write, Aria processes, Prim archives | Agent-to-Aria communication queue |
| `state/aria_today.md` | Aria | Daily scratch pad. Cleared at 7am wake. |
| `state/agent_manifest.md` | Architect | Current prompt versions. Prim reads for verification. |
| `state/trigger.md` | Aria writes, Iris executes, Prim archives | Iris trigger queue |
| `state/outbound/[agent].md` | Aria writes, agent deletes | Aria-to-specialist messages |
| `state/luna_notification.md` | Luna writes, send script reads | Tonight's Luna notification content |
| `state/luna_send_time.md` | Aria writes, Luna reads/deletes | Override send time for Luna |
| `state/aria_calendar_watch.md` | Aria | Pending late-night calendar events |
| `state/vera_watch.md` | Aria | Pending doctor appointments with trigger status |

---

## Data Tiers

### Hot (`data/hot/`) — 4 to 8 weeks, full detail
Active data agents read for current work: nutrition daily reports, fitness tracker data, sleep records, training logs, race or event research. Prim manages the boundary — old hot data is moved to warm on the weekly run.

### Warm (`data/warm/`) — up to 12 months, summarized
Pre-summarized by week or month. Medications list, supplements list, any medical baselines. Read occasionally when trend context is needed.

### Cold (`data/cold/`) — historical, source of truth
Raw data exists here but agents never read it directly. Only summaries and insights from cold are read. Archived knowledge base entries, archived flags. Prim moves data here and never reads the content.

---

## Scheduling Model

> **Customizer note:** Adjust times to fit your life. Times below are the defaults. Prim runs at 3am because the user may be active late — 3am is effectively guaranteed clear.

| Time | Task |
|------|------|
| Daily 3am Sun–Fri | Prim nightly run |
| Daily 7am | Aria morning check-in |
| Daily 8:15am | Flora daily nutrition |
| Daily 7pm | Aria evening check-in |
| Daily 9:30pm | Luna evening touchpoint |
| Daily 9am / 1pm / 6pm | Iris trigger queue |
| Monday 8:30am | Flora weekly reconciliation |
| Wednesday 3:30am | Soleil weekly check-in |
| 1st & 15th 5am | Vita biweekly research |
| Saturday 3am | Prim weekly run |
| Saturday 4am | Agent batch |
| Saturday 11pm | Lyra weight summary |
| Sunday 3am | Aria weekly synthesis |
| Sunday 8pm | Aria weekly report |

---

## Prompt Versioning

All agent prompts carry a version number. Format: `Major.Minor`.

**Major bump** — fundamental redesign of role, voice, or approach.
**Minor bump** — additions, tweaks, new sections, rule changes.

Version location: second line of each prompt header: `### Health System Agent Prompt vX.Y`

Agents stamp their version on all output. `state/agent_manifest.md` is the architect-maintained source of truth for current versions. Prim reads it on her Saturday weekly run for verification.

---

## File Storage

The system lives in iCloud Drive (or any folder shared across your devices). Cowork mounts the folder directly.

**All scheduled tasks run on a single designated machine.** Do not run scheduled tasks on multiple machines — the system is designed around a single writer for automated runs.

**git is used for version control of agent prompts and architecture files only.** No health data ever goes in git. The `.gitignore` covers `state/`, `data/`, `reports/`, and `sessions/`.
