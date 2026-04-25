# data/

Health data. Not version controlled — excluded by .gitignore.

## Tiers

**`hot/`** — Last 4–8 weeks, full detail. Active data agents read for current work.
- `nutrition/` — Daily nutrition reports from [NUTRITION_APP]
- `training/` — Fitness tracker and strength training data
- `sleep/` — Sleep data from tracker and any sleep devices
- `garmin/` — Full fitness tracker account export (if applicable)
- `races/` — Upcoming event research files

**`warm/`** — Up to 12 months, summarized. Read occasionally for trend context.
- `medications.md` — Current medications list
- `supplements.md` — Current supplements list
- Any medical baselines (DEXA, labs, etc.)

**`cold/`** — Historical source of truth. Agents never read directly.
- `flag-archive/` — Prim's archived closed flags by month
- `[agent]/` — Archived knowledge base entries per agent (Prim manages)
- Historical data exports

## Prim's Responsibility
Prim moves data between tiers on her Saturday weekly run — by date metadata only. She never reads or summarizes content.
