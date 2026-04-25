<\!-- TEMPLATE: Replace [PLACEHOLDERS] with your personal details. See CUSTOMIZE.md for guidance. -->

# Health System Template

A personal health management system built on Claude (Cowork / Claude Agent SDK). 13 specialist agents plus a coordinator, running as independent Cowork sessions, coordinated through shared files in iCloud Drive.

This is personal infrastructure, not a product. Clone it, fill in your details, and make it yours.

---

## How It Works

**Aria** is the only daily interface. She delivers morning check-ins (7am), evening check-ins (7pm), and a Sunday weekly report. The 13 specialist agents — cardio coach, nutritionist, sleep coach, strength coach, and others — work in the background. They file findings through a shared flag queue. Aria synthesizes everything and surfaces what matters.

Agents are persistent. They maintain session logs and knowledge bases that survive across sessions. The worst-case data loss on an interrupted session is the current topic since the last topic-break write.

Everything runs on a schedule. Scheduled tasks fire on a single designated machine. iCloud Drive (or your shared folder) keeps files in sync across devices.

---

## The Agents

| # | Name | Role |
|---|------|------|
| 1 | **Aria** | Coordinator — your only daily interface |
| 2 | **Pace** | Cardio Coach |
| 3 | **Brynn** | Strength Coach |
| 4 | **Flora** | Nutrition |
| 5 | **Luna** | Sleep Coach |
| 6 | **Vera** | Doctor's Assistant |
| 7 | **Soleil** | Mental Health |
| 8 | **Lumi** | Personal Care |
| 9 | **Seren** | Sexual Health |
| 10 | **Vita** | Longevity |
| 11 | **River** | Hydration, Recovery & Cross-Training |
| 12 | **Lyra** | Weight & Body Composition |
| 13 | **Prim** | System Operations (infrastructure only) |
| 14 | **Iris** | Reactor (trigger queue executor) |

---

## Getting Started

### Step 1 — Clone and customize

```bash
git clone [THIS_REPO_URL]
cd health-system-template
```

Open `CUSTOMIZE.md` and work through every `[PLACEHOLDER]`. That file lists every field you need to fill in, what it means, and where it appears.

### Step 2 — Connect Cowork

Open Cowork and mount your Health Manager folder. The system assumes Cowork has read/write access to the folder.

### Step 3 — Start a session

Open a new Cowork session. The entry point for every session is `dispatch.md`. Paste its contents to start, or configure Cowork to read it automatically.

To start as the Architect (to configure the system), enter "architect" or "1" when the dispatcher asks who you want to work with.

### Step 4 — Set up scheduled tasks

All scheduled tasks run on a single designated machine. In Cowork, create scheduled tasks for each agent using the schedule defined in `ARCHITECTURE.md`. Each task should read the relevant agent's prompt file (`agents/[name].md`) and execute.

> **Note:** Do not run scheduled tasks on multiple machines. The system is designed around a single writer for automated runs.

### Step 5 — Initialize state files

Run an Architect session. The Architect will help you initialize:
- `state/agent_manifest.md` — current prompt versions
- `state/health_state.md` — initial health snapshot
- `state/flags.md` — empty flag queue
- `state/aria_today.md` — empty daily scratch pad
- `data/warm/medications.md` and `data/warm/supplements.md`

---

## Folder Structure

```
Health Manager/
├── agents/                    All 14 agent prompts
│   └── archive/               Old prompt versions
├── state/                     Live system state (not in git)
│   └── outbound/              Aria-to-specialist messages
├── data/                      Health data (not in git)
│   ├── hot/                   Last 4-8 weeks, full detail
│   ├── warm/                  Last 12 months, summarized
│   └── cold/                  Historical, source of truth
├── sessions/logs/             Agent session logs and knowledge bases (not in git)
├── reports/                   Generated reports (not in git)
├── schedules/                 Schedule files for Iris
├── inbox/                     Drop zone for raw data exports
├── scripts/                   Operational scripts (e.g., notification sender)
├── architect/                 Architect working files
│   ├── architect.md           Start any architect session with this
│   ├── decisions/             Architecture Decision Records (ADRs)
│   ├── flows/                 Key workflow documentation
│   └── docs/                  Reference documentation
├── dispatch.md                Entry point for all sessions
└── .gitignore                 Excludes data/, state/, reports/, sessions/
```

---

## Key Design Principles

- **Agent Isolation** — each agent runs in its own session; no impersonation
- **Single Writer Per State** — every shared file has exactly one authorized writer
- **Lane Discipline** — health state (Aria) and infrastructure state (Prim) never mix
- **Sessions Have No Reliable Endings** — topic-break writes are the floor, not the ceiling
- **Prim Is Operations** — everything you'd manually monitor eventually falls to Prim
- **Research First** — agents gather data before asking the user anything
- **Medical Boundary** — agents prepare and surface; doctors decide

Full rationale for each principle is in `architect/decisions/`.

---

## Version Control

Git tracks agent prompts and architecture files only. Health data never goes in git. The `.gitignore` excludes `state/`, `data/`, `reports/`, and `sessions/`.

---

## Customization

See `CUSTOMIZE.md` for a complete guide to every placeholder in this template.
