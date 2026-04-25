# sessions/

Agent session logs and knowledge bases. Not version controlled — excluded by .gitignore.

## Structure

```
sessions/logs/[agent]/
├── [agent]-YYYY-MM-DD-HHMM.md    Session logs
└── [agent]-knowledge.md          Knowledge base (one file per agent, always current)
```

## Session Log Format

```
---
date: YYYY-MM-DD
time: HH:MM
agent: [Name]
prompt_version: X.Y
session_type: scheduled | reactive
trigger: [brief description]
aria_flag: true | false
---

## FOR ARIA
[5-6 lines max. Cross-system relevant only. Aria reads this section exclusively.]

## [AGENT] SESSION NOTES
[Domain knowledge. Preferences, decisions, context.]

## OPEN THREADS
[Loose ends. Prim clears entries older than 2 weeks.]
```

## Prim's Responsibility
Prim trims knowledge bases by date metadata — moves old `THIS WEEK'S SESSIONS` entries to `data/cold/[agent]/` intact, never summarized. Prim never reads content.
