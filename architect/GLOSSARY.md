<!-- TEMPLATE: Replace [PLACEHOLDERS] with your personal details. See CUSTOMIZE.md for guidance. -->

# GLOSSARY

Key terms used throughout the health system. When in doubt, this is the canonical definition.

---

**agent batch**
The Saturday 4am scheduled run where all 13 specialists fire simultaneously and file weekly inputs for Aria's Sunday synthesis. Prim runs at 3am to clean before the batch fires.

**agent manifest** (`state/agent_manifest.md`)
The architect-maintained file listing the current prompt version for every agent. Prim reads it during her Saturday weekly run for version verification. The architect updates it in the same commit as any prompt version bump — never as a separate step.

**aria_flag**
A boolean field in a session log's YAML frontmatter (`aria_flag: true | false`). When `true`, Prim writes a flag entry in `state/flags.md` on her next nightly run so Aria picks it up. The agent sets this field when the session contains something cross-system relevant.

**aria_today** (`state/aria_today.md`)
Aria's daily scratch pad. She writes trivial-but-useful context for the next check-in: dinner plans the partner mentioned, a gym skip, a topic to come back to. Not health state. Cleared at end of the 7am wake only — giving 7pm notes a full overnight window to reach the next morning's Aria.

**architect session**
A Cowork session started by pasting `architect.md`. The architect designs and maintains the system — writes and edits agent prompts, manages the scheduled task configuration, updates the manifest. The architect does not report on health data.

**backfill scan**
Flora's 14-day lookback on every daily 8:15am run. For each date in the window, she checks whether a valid data file exists. If not, she searches Gmail for the [NUTRITION_APP] email for that date. Handles late-arriving emails and missed daily runs.

**cold** (`data/cold/`)
The historical data tier. Raw data exists here as source of truth but agents never read it directly — only summaries and insights from cold are read. Prim moves data here from warm. Examples: weight history CSV, archived [NUTRITION_APP] logs, archived knowledge base entries, archived flags.

**confirm-as-is**
When the user tells Aria at check-in that a Flora suspicious-day flag is accurate ("that day was a fast, it's fine"), Aria relays the confirmation to Flora via `state/outbound/flora.md`. Flora stamps the data file `user_confirmed_as_is: true` on her next wake and closes the flag. No Flora session needed.

**coordinator**
Aria's role. She is the user's only daily interface. She synthesizes all specialist inputs, resolves cross-agent conflicts, owns the training schedule, and delivers check-ins and the weekly report.

**decisions/** (`decisions/`)
The folder of Architecture Decision Records (ADRs). Each ADR captures a key design decision: context, decision, and consequences. ADRs are never deleted — superseded decisions get a new ADR and the old one is marked `Superseded`.

**distraction layer**
Step 1 of Aria's wake protocol. She opens every check-in with a a question based on one of the user's stated interests — delivered before any reads. the user reads and thinks while Aria loads context silently through steps 2–6.

**flag** (`state/flags.md`)
The primary channel from specialists to Aria. Each flag is a YAML frontmatter block with: agent, timestamp, priority (routine/attention/urgent), action (FYI/schedule session/surface to the user now), status (open/closed), and a note. Aria processes all open flags on every wake. Prim archives closed flags on her Saturday run.

**FOR ARIA section**
The first section of every agent session log (`## FOR ARIA`). 5–6 lines max. Cross-system relevant only. Aria reads this section exclusively — she never reads the full session log. The full log is the specialist's domain record.

**health_state.md** (`state/health_state.md`)
Aria's live health snapshot. Sole writer: Aria. Updated at every wake. Contains: current health priorities, active flag summary, domain summaries from each specialist. Health only — infrastructure state belongs in `system_status.md`. The first thing every specialist reads on wake.

**hot** (`data/hot/`)
The most recent 4–8 weeks of data, at full detail. Agents read this for active work. Flora writes nutrition files here. Prim moves old hot data to warm on her Saturday run.

**knowledge base**
Every specialist maintains `sessions/logs/[agent]/[agent]-knowledge.md`. Read on every interactive wake. Contains: domain preferences, current targets, this week's sessions, patterns the agent is tracking, open threads. Prim trims old entries to cold storage by date — never reads content.

**lane discipline**
The principle that each file type stays in its owner's domain. `health_state.md` is health only (Aria's lane). `system_status.md` is infrastructure only (Prim's lane). Agents write only to files in their lane.

**manual entry**
The Flora workflow for correcting a past day's nutrition data. the user provides data in any format (conversation, screenshot, description). Flora parses, reads back a proposed diff, and writes only after the user confirms. Files get a `processed_by: Flora-manual-entry` stamp. See `flows/manual-entry.md`.

**nightly run**
Prim's daily 3am run (Sunday through Friday). Covers: `health_state.md` staleness check, session counting, aria_flag processing, outbound monitoring, aria_today monitoring, `system_status.md` snapshot write, nightly log. Distinct from the weekly run (Saturday 3am).

**outbound** (`state/outbound/[agent].md`)
The Aria-to-specialist message channel. A file only exists when Aria has a pending message for that specialist. The specialist reads and deletes the file on their next wake. Aria is the sole writer. Files unread for 3+ days are flagged by Prim.

**prompt version**
Every agent prompt carries a `Major.Minor` version number in its second header line. Agents stamp `prompt_version: X.Y` on all YAML frontmatter output and `— [Agent Name] vX.Y` on prose output. The architect bumps the version on every edit and updates `state/agent_manifest.md` in the same commit. See ADR-008.

**provenance**
The set of frontmatter fields on a nutrition data file that record how it was written: `processed_by`, `corrected_from`, `corrected_at`, `source_format`, `user_confirmed_as_is`, `confirmed_at`, `confirmed_source`. Provenance on manual-entry files prevents the weekly [NUTRITION_APP] reconciliation from silently overwriting the user's corrections.

**Single Writer Per State**
The design principle that every shared file has exactly one agent authorized to write it. See ADR-004 and the writer table in `ARCHITECTURE.md`.

**session log**
The structured file every specialist writes after every meaningful interactive session: `sessions/logs/[agent]/[agent]-YYYY-MM-DD-HHMM.md`. YAML frontmatter + FOR ARIA section + agent notes section + open threads. Automated scheduled tasks do not generate session logs — they write to data files and flags directly.

**suspicious-day detection**
Flora's automated scan of the 14-day window on every daily run. Five hard triggers plus a judgment layer. Flags days that look incomplete. Flags stay open until the user corrects or confirms. See `flows/daily-nutrition-pipeline.md`.

**system_status.md** (`state/system_status.md`)
Prim's infrastructure snapshot. Sole writer: Prim. Overwritten nightly. Contains: agent prompt versions reference (pointer to manifest), last Prim run, data pipeline state, flag queue counts, aria_today state, outbound queue state. Aria reads it when she needs infrastructure context during synthesis — she never mirrors it into `health_state.md`.

**topic-break write**
A silent write by an interactive agent to their session log and knowledge base at a natural pause in the conversation. the user never sees it happen. The purpose: bound data loss if the session ends abruptly. The worst case is losing the current topic since the last topic-break write. See ADR-005.

**W command**
Any interactive agent session can be closed immediately by the user typing `W` (or `w`) alone. The agent writes and closes in one action, confirms in one line in their own voice.

**warm** (`data/warm/`)
Pre-summarized data covering the last 12 months. Agents read this occasionally for trend context. Examples: weekly nutrition averages, monthly training summaries, medications list, supplements list, health baseline. Prim moves old warm data to cold on her Saturday run.

**weekly run**
Prim's Saturday 3am run. Everything in the nightly run, plus: flag archival, `health_state.md` trim, knowledge base trim, agent version verification, weekly log. Runs before the Saturday 4am agent batch.
