<!-- TEMPLATE: Replace [PLACEHOLDERS] with your personal details. See CUSTOMIZE.md for guidance. -->

# Health System Template

A comprehensive personal health management system powered by Claude, featuring 12 AI agents that work together to manage different aspects of your health.

## What Is This?

This is a production-grade health management system originally built for an individual, now open-sourced as a template. It consists of:

- **12 specialist agents** — each with a distinct personality, expertise, and role
- **1 coordinator agent** — who orchestrates the others and interfaces with you
- **Automated workflows** — scheduled tasks that run on your behalf
- **Structured data management** — with a tiered system (hot/warm/cold) for efficiency
- **Deep integration** — with your calendar, email, fitness apps, medical data, and more

Each agent is designed to work autonomously, research first before asking, and resolve conflicts before anything reaches you. The system requires Claude, Google Calendar, Gmail, and (ideally) a file system with iCloud, Dropbox, or similar cloud storage.

## The 12 Agents

| # | Name | Role | Personality |
|---|------|------|-------------|
| 1 | Aria | Coordinator | Your morning and evening interface; orchestrates all other agents |
| 2 | Pace | Running Coach | Trains you for endurance events; performance-focused |
| 3 | Brynn | Strength & Mobility | Gym coach; no-nonsense, direct feedback on your body |
| 4 | Flora | Nutrition | Culinary expert; opinionated about food and fuel |
| 5 | Luna | Sleep Coach | Evening presence; makes rest irresistible |
| 6 | Vera | Doctor's Assistant | Prepares for medical appointments; bridges medicine and you |
| 7 | Soleil | Mental Health | Sister voice; notices what you haven't noticed |
| 8 | Lumi | Personal Care | Overly familiar; expert in grooming and hygiene |
| 9 | Seren | Sexual Health | Matter-of-fact; brings the agenda |
| 10 | Vita | Longevity | Works backward from mortality; protecting your future |
| 11 | River | Hydration & Recovery | Quiet presence; rest as work |
| 12 | Lyra | Weight & Body Composition | Theatrical accountability; celebrating progress |

## How It Works

### The Daily Cycle

- **7:00 AM** — Aria delivers your morning check-in (5 minutes)
- **8:15 AM** — Flora analyzes yesterday's food logging (automated)
- **7:00 PM** — Aria delivers your evening check-in (5 minutes)
- **10:30 PM** — Luna invites you to bed (Luna only; her unique direct touchpoint)

### Weekly Tasks

- **Monday 8:30 AM** — Flora reconciles weekly nutrition data
- **Wednesday Noon** — Soleil checks your mental health patterns
- **Saturday 9 PM** — All agents batch their data for the week
- **Saturday 11 PM** — Lyra delivers your weight trend summary
- **Sunday 6 PM** — Aria synthesizes everything into a weekly report

### As-Needed Sessions

Medical appointments, major training blocks, lifestyle changes, or anything that needs depth beyond 5-minute check-ins triggers a dedicated 20-30 minute session.

## System Design Principles

**Research First** — Agents investigate data and history before asking you anything

**Ask Often, Ask Well** — Prepare findings, present them, ask clear questions

**One Question at a Time** — Never stack questions

**Coordinator Resolves Conflicts** — You only see consensus or clear, explained disagreement

**Goal-Oriented, Not Streak-Obsessed** — Coming back is always progress

**Pragmatic Replanning** — Replan before you break something

**Medical Boundary** — Agents prepare and inform; doctors decide

**Token Efficiency** — Small state file first, tiered data, never raw history

## Getting Started

### 1. Fork or Clone This Repo

### 2. Customize Your Details

Use **CUSTOMIZE.md** to fill in all placeholders:
- Personal info (name, age, location)
- Medical profile (conditions, medications, doctors)
- Devices and tools you use
- Calendar and email accounts
- Goals and context

### 3. Set Up Your File System

Create this folder structure (in [CLOUD_STORAGE] or your system):

```
Health System/
├── agents/
│   ├── bootstrap.md (with your details filled in)
│   ├── aria.md through vita.md (12 agent prompts)
├── state/
│   └── health_state.md (coordinator's live snapshot)
├── data/
│   ├── hot/ (last 4-8 weeks, detailed)
│   ├── warm/ (last 12 months, summarized)
│   └── cold/ (historical archives)
├── reports/
├── sessions/
└── dashboard.html
```

### 4. Add Calendar and Email Access

- Share your primary calendar with Claude (read access)
- Grant Gmail access for automated food logging and email parsing

### 5. Set Up Scheduled Tasks

Use Claude Code's scheduled tasks feature to run:
- `aria-morning-checkin` (daily 7am)
- `aria-evening-checkin` (daily 7pm)
- `flora-daily-nutrition` (daily 8:15am)
- `luna-evening-touchpoint` (daily 10:30pm)
- And others as needed

See **bootstrap.md** for the full schedule.

### 6. Start a Session

Paste the **bootstrap.md** content at the start of any session with Claude to give the Architect context.

## Key Design Features

### Token Efficiency

The system uses a small **health_state.md** file (~500-1000 words) as the coordinator's live snapshot. Every agent reads this first. This covers 95% of routine needs without loading large files.

Detailed data is tiered:
- **Hot data** (4-8 weeks) — detailed, used actively
- **Warm data** (12 months) — pre-summarized, read occasionally
- **Cold data** (historical) — insights only, never raw

### Automation Without Surveillance

The system is designed for **Claude running in Cowork mode**, where scheduled tasks run on a computer that stays on. It never monitors you in real-time. It:

- Reads data you explicitly log (food, weight, sleep from devices)
- Checks your calendar when scheduling or planning
- Processes email reports you've subscribed to
- Runs on a schedule you control

You are never watched. You are always in control.

### Personality Over Generic Coaching

Each agent has a complete personality — not just a role. Brynn is blunt about your body. Flora loves food. Luna makes sleep irresistible. Lyra is funny. This isn't fluff — personality is what makes you *want* to engage with the system.

The personalities are described in detail in each agent prompt. Read them. They are the heart of this system.

### Research-First Architecture

Before any agent asks you a question, they:

1. Check your data and history
2. Consult the coordinator's state file
3. Research relevant science or patterns
4. Prepare findings
5. Only then ask

This saves your time and respects your attention.

## Customization Guide

Everything in this template is designed to be customizable:

- **Agent personalities** — Modify how agents speak, what they prioritize, what they care about
- **Scheduling** — Change check-in times, add or remove agents, adjust frequencies
- **Tools and apps** — Replace [FOOD_TRACKING_APP] with whatever you actually use
- **Medical context** — Update with your conditions, medications, and doctor information
- **Goals** — Adapt the weight loss, running, strength, or longevity goals to yours
- **Life context** — Change relationships, employers, locations, constraints

This is a *template*. It should reflect your life, not the other way around.

## What You'll Need

- **Claude** (Claude Code for scheduled tasks, Claude for chat sessions)
- **Google Calendar** (shared with Claude, read access)
- **Gmail** (for automated email processing)
- **Cloud storage** (iCloud, Dropbox, or local file system)
- **Fitness tracking app** (Garmin, Apple Watch, Strava, Hevy, or others)
- **Food logging app** (optional but very useful)
- **A computer that can stay on** (for scheduled tasks)

## How to Use This System

### For Getting Started

1. Read this README
2. Read **CUSTOMIZE.md** carefully
3. Fill in all [PLACEHOLDERS] in your copies of the agent files
4. Read the **bootstrap.md** file — it explains the full system architecture
5. Read 2-3 agent prompts to get a feel for the personality and depth

### For Daily Use

- **Morning (7am):** Aria check-in
- **Evening (7pm):** Aria check-in
- **10:30pm:** Luna's evening touchpoint
- **As needed:** Request a dedicated session with an agent through Aria

### For Maintenance

- Update [CLOUD_STORAGE] paths if you change your folder structure
- Review and update **bootstrap.md** quarterly as your context evolves
- Archive old data from /hot/ to /warm/ regularly
- Keep agent prompts fresh — they should evolve as you do

## A Note on Privacy and Data

This system:

- **Does not** monitor you in real-time
- **Does not** send data to external services (beyond what you explicitly configure)
- **Does** read your calendar and email (because you configure that access)
- **Does** require you to trust Claude with your health data

If you're uncomfortable with any of that, customize it. You can:

- Run it entirely on local files without cloud storage
- Limit calendar access to only certain calendars
- Remove email processing and do manual data entry instead
- Disable certain agents entirely
- Run the system less frequently

This is your system. Build it the way you're comfortable.

## Extending the System

This template includes 12 agents. You can:

- **Add more agents** — Create a new agent prompt for anything not covered (physical therapy, medication adherence, habit building, etc.)
- **Modify existing agents** — Change what they track, how often they check in, what they prioritize
- **Integrate more tools** — Connect Fitbit, Oura Ring, Withings Scale, or other health apps
- **Build custom dashboards** — Create a web interface that shows your system state

The architecture is designed to be extensible. Each agent is independent. Add more.

## Troubleshooting

**An agent asks the same question every time** — Update health_state.md with the answer. Agents read the state file first.

**Scheduled tasks aren't running** — Make sure your computer stays on and Claude has permission to run them.

**Email processing isn't working** — Check that Claude has Gmail access and that your food app is sending emails to the right address.

**I'm not using this system** — That's okay. It's designed to be low-friction (5 minutes a day), but it only works if you show up. Start with Aria's check-ins. Everything else follows.

## Attribution and License

This template is a sanitized version of a personal health system. It's designed to be forked, customized, and made your own.

Build it. Use it. Extend it. Share what you learn.

---

**Next step:** Read **CUSTOMIZE.md** to fill in your personal details, then read **bootstrap.md** to understand the full system architecture.
