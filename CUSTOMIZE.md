<\!-- TEMPLATE: Work through this file top to bottom. Every [PLACEHOLDER] is explained here. -->

# Customization Guide

This file walks you through every placeholder in the template. Work through it once and the system is ready to personalize.

---

## Identity Placeholders

### `[USER_NAME]`
Your name — or whatever you want the agents to call you. Used throughout all agent prompts.

**Appears in:** `agents/aria.md`, `agents/pace.md`, `agents/brynn.md`, `agents/flora.md`, `agents/luna.md`, `agents/vera.md`, `agents/soleil.md`, `agents/lumi.md`, `agents/seren.md`, `agents/vita.md`, `agents/river.md`, `agents/lyra.md`

### `[USER_AGE]`
Your current age.

### `[USER_GENDER]`
Your gender — used for medical context and how agents frame recommendations.

### `[PARTNER_NAME]` *(optional)*
Your partner's name, if relevant. Used in Aria, Soleil, and Seren for household and relationship context. Remove or leave as `[PARTNER_NAME]` if not applicable.

---

## Account Placeholders

### `[YOUR_EMAIL]`
Your email address. Used by Flora to receive nutrition app daily/weekly report emails.

### `[YOUR_CALENDAR_EMAIL]`
Your calendar email address. Aria reads this calendar for scheduling decisions, late-night event detection, and doctor appointment detection.

### `[YOUR_TIMEZONE]`
Your timezone in IANA format. Example: `America/Chicago`, `America/New_York`, `Europe/London`.

**Used in:** Every agent's `TZ=[YOUR_TIMEZONE] date` bash command that establishes the current time.

---

## App Placeholders

### `[NUTRITION_APP]`
The app you use to track food. Examples: `Lose It`, `MyFitnessPal`, `Cronometer`.

Flora's automated pipeline assumes this app sends daily email summaries to `[YOUR_EMAIL]`. If your app doesn't send emails, adapt Flora's data pipeline section to your actual data source.

### `[NUTRITION_APP_EMAIL_SUBJECT_PATTERN]`
The subject line pattern of your nutrition app's daily email. Flora uses this to search Gmail.
Example for Lose It: `"Daily Report for [Month Day, Year]"`

### `[STRENGTH_LOGGING_APP]`
The app you use to log strength training. Examples: `Hevy`, `Strong`, `Strava`.

Brynn reads training data from `data/hot/training/`. If your app doesn't sync to Strava/Garmin, you'll need to drop exports into `inbox/` manually.

---

## Health Profile Placeholders

These appear in agent health profile sections. Fill in what applies; remove what doesn't.

### `[CURRENT_WEIGHT]` and `[GOAL_WEIGHT]`
Used by Lyra for weight trend context and goal framing.

### `[LIST_CONDITIONS]`
Your active medical conditions. Used by Aria and Vera for health context.

### `[LIST_MEDICATIONS]`
Your current medications. Full list lives in `data/warm/medications.md`. The agents reference this file rather than storing the full list in prompts.

### `[LIST_DOCTORS_AND_FREQUENCY]`
Your medical team with how often you see each. Example: `PCP (3-4x/year), Cardiologist (1x/year)`.

### `[YOUR_PRIMARY_HEALTH_CONCERN]`
The #1 health priority the system should never let go quiet. Example: `cardiovascular health`, `metabolic health`, `musculoskeletal strength`.

Used by Aria to frame the primary mission. Every week she checks whether the work is being done that moves this needle.

---

## Fitness Placeholders

### `[CARDIO_TYPE]`
Your primary cardio modality. Examples: `running`, `cycling`, `swimming`, `rowing`.

Pace is framed around this. If you do multiple cardio types, list the primary one here and note the others.

### `[LIFTING_DAYS]`
Your typical lifting schedule. Example: `Monday, Wednesday, Friday`.

### `[HOME_EQUIPMENT]`
What strength equipment you have at home. Example: `bodyweight and resistance bands`, `dumbbells (up to 40 lbs)`.

### `[LUNA_NOTIFICATION_TIME]`
The time Luna sends her evening notification. Default: `9:30pm`. Adjust to match your typical wind-down window.

### `[TARGET_BEDTIME]`
The bedtime you're aiming for. Luna uses this to frame success.

---

## Goals Placeholders

### `[SECONDARY_GOAL]` and `[TERTIARY_GOAL]` (in Brynn)
Brynn's mission, in order of priority. The template has placeholders for goals 2 and 3. Fill in what applies: body composition, cardiovascular fitness, strength benchmarks, etc.

### `[PRIMARY_LONGEVITY_CONCERN]` (in Vita)
The longevity domain Vita should treat as most urgent for your profile. Examples: `cardiovascular health`, `metabolic resilience`, `musculoskeletal strength`.

---

## Medication-Specific Placeholders

Several agents have notes like `[YOUR_MEDICATIONS]` or `[MEDICATION] context`. These are places to add protocol notes for any medications that affect training, nutrition, recovery, or sexual health:

- **Pace:** race-week medication protocols
- **Brynn:** medications affecting muscle retention
- **Flora:** appetite effects, event-week protocols
- **River:** medications affecting thirst or hydration signals
- **Seren:** medications with sexual health effects

Fill in what applies. Remove sections that don't.

---

## After Filling In Placeholders

1. Run a full-text search for `[` in the agents folder — this catches any remaining unfilled placeholders.
2. Start an Architect session and work through system initialization.
3. Set up scheduled tasks in Cowork for each agent.
4. Run your first Aria morning check-in and verify everything is working.

---

## What Not to Change

- Agent names (Aria, Pace, Brynn, Flora, Luna, Vera, Soleil, Lumi, Seren, Vita, River, Lyra, Prim, Iris) — these are referenced throughout the system
- File paths in agent prompts — these match the folder structure; only change them if you restructure the folder
- The W command — this is a system-wide emergency exit; don't change it
- The session log and knowledge base formats — Prim reads these by metadata structure
