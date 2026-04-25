# Health System

A personal health management system built on Claude (Cowork / Claude Agent SDK). 13 specialist agents plus a coordinator, running as independent Cowork sessions, coordinated through shared files in iCloud Drive.

This is personal infrastructure, not a product. It is designed around one person's health profile, devices, and life constraints.

---

## How It Works

**Aria** is the only daily interface. She delivers morning check-ins (7am), evening check-ins (7pm), and a Sunday weekly report. The 13 specialist agents — running coach, nutritionist, sleep coach, strength coach, and others — work in the background. They file findings through a shared flag queue. Aria synthesizes everything and surfaces what matters.

Agents are persistent. They maintain session logs and knowledge bases that survive across sessions. Aria reads the relevant excerpts on every wake. The worst-case data loss on an interrupted session is the current topic since the last topic-break write.

Everything runs on a schedule. Scheduled tasks fire on the Mac Studio. iCloud Drive keeps files in sync across devices.

---

## Repository Contents

This repo tracks agent prompts and architecture files only. No health data is ever committed.

```
agents/          All 13 agent prompts + Aria
decisions/       Architecture Decision Records
docs/            Reference documentation
  agents/        Per-agent reference (roles, inputs, outputs)
flows/           Key workflow documentation
architect.md     Session start file — read this to begin any architect session
ARCHITECTURE.md  System design overview
CHANGELOG.md     Notable changes, most recent first
GLOSSARY.md      Key terms and concepts
README.md        This file
```

**Not in this repo (gitignored):**
- `state/` — live system state (health_state.md, flags.md, system_status.md, etc.)
- `data/` — all health data (nutrition, training, sleep, medical baselines, etc.)
- `reports/` — generated weekly reports and appointment notes
- `sessions/` — agent session logs and knowledge bases

---

## Starting an Architect Session

Open a new Cowork session and paste the contents of `architect.md`. The architect file contains the full system context, open action items, and session start protocol.

---

## Starting an Agent Session

Open a new Cowork session and paste the contents of `agents/[name].md`. Each agent prompt is self-contained — it includes the wake protocol, knowledge base location, write protocol, and flag format.

---

## Scheduled Tasks

All scheduled tasks run on the Mac Studio. The schedule is documented in `ARCHITECTURE.md` and in the `architect.md` scheduling model section. Do not run scheduled tasks on the MacBook — the Mac Studio is the single writer for automated runs.

---

## Public Template

A sanitized version of this system (with `[PLACEHOLDERS]` replacing all personal data) is available at:
`[YOUR_TEMPLATE_REPO_URL]`

---

## Key Design Principles

- **Agent Isolation** — each agent runs in its own session; no impersonation
- **Single Writer Per State** — every shared file has exactly one authorized writer
- **Lane Discipline** — health state (Aria) and infrastructure state (Prim) never mix
- **Sessions Have No Reliable Endings** — topic-break writes are the floor, not the ceiling
- **Prim Is Operations** — everything the architect would manually monitor falls to Prim

Full rationale for each principle is in `decisions/`.
