# state/

Live system state files. Not version controlled — excluded by .gitignore.

| File | Owner | Purpose |
|------|-------|---------|
| `health_state.md` | Aria | Live health snapshot. Health only. Updated every wake. |
| `system_status.md` | Prim | Infrastructure snapshot. Overwritten nightly. |
| `flags.md` | Agents write, Aria processes, Prim archives | Agent-to-Aria communication queue |
| `aria_today.md` | Aria | Daily scratch pad. Cleared at 7am wake. |
| `agent_manifest.md` | Architect | Current prompt versions for all agents. |
| `trigger.md` | Aria writes, Iris executes | Iris trigger queue |
| `luna_notification.md` | Luna | Tonight's notification content |
| `luna_send_time.md` | Aria writes, Luna reads | Override send time for Luna |
| `aria_calendar_watch.md` | Aria | Pending late-night calendar events |
| `vera_watch.md` | Aria | Pending doctor appointments with trigger status |
| `outbound/` | Aria writes, specialists delete | Aria-to-specialist messages |

Files in `outbound/` only exist when Aria has a pending message for that specialist.
