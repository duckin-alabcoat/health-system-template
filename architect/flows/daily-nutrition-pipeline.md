# Flow: Daily Nutrition Pipeline

The daily nutrition pipeline runs every day at 8:15am. It is the most data-dependent flow in the system — it cannot move earlier because it depends on the [NUTRITION_APP] email arriving at 8am.

---

## Trigger

Cowork scheduler fires **Flora** at 8:15am.

---

## Steps

### 1. Outbound check
Flora reads `state/outbound/flora.md` if present. If the file contains a confirm-as-is entry from Aria, Flora stamps the relevant data file (`user_confirmed_as_is: true`, `confirmed_at`, `confirmed_source: aria-relay`), closes the open flag for that date in `state/flags.md`, and deletes the outbound file. Then proceeds.

### 2. Backfill scan (14-day window)
Flora checks whether each of the last 14 calendar days has a valid data file at `data/hot/nutrition/nutrition_app_daily_YYYY-MM-DD.md`. A file is invalid if it is missing, empty, or missing required frontmatter fields. Gaps are collected.

### 3. Gap filling
For each gap date, Flora searches Gmail for the [NUTRITION_APP] daily email (`from:donotreply@nutrition_app.com`). If the email is found, she parses it and writes the file (step 5). If not found, she skips silently — [NUTRITION_APP] emails sometimes arrive 1–2 days late and the 14-day window will catch them.

### 4. Today's email
Flora searches Gmail for yesterday's [NUTRITION_APP] daily report. If not found yet, she stops — no flag, no error. Tomorrow's backfill scan will pick it up.

### 5. Parse and write
Flora parses the HTML email: summary stats, meal-by-meal log, full nutrient breakdown, Garmin data. Writes to `data/hot/nutrition/nutrition_app_daily_YYYY-MM-DD.md` with YAML frontmatter.

**Nothing-logged days:** If the email says "nothing logged," Flora writes the file with `calories: 0` and nutrients as `not logged` (not `0`) — preserving the distinction between zero intake and no data.

### 6. Analysis
Flora analyzes the day against targets: protein vs [PROTEIN_TARGET]g, fiber vs [FIBER_TARGET]g, calories vs [CALORIE_TARGET] (RMR-based), sodium over [SODIUM_LIMIT]mg. Skips any `not logged` field.

### 7. Suspicious-day scan
Flora checks each of the 14 data files for suspicious-day conditions:
- File has `calories: 0` and `protein: not logged` (nothing logged)
- Any main meal slot empty while others have entries
- Total calories < 800 on a non-fasting day
- Protein < 50g with total calories > 1,000
- Weight recorded but zero food logged

For each suspicious date: if the file already has `user_confirmed_as_is: true` or `processed_by: Flora-manual-entry`, skip. If an open Flora flag already exists for this date in `state/flags.md`, leave it in place (no duplicate). Only write a new flag if none exists.

### 8. Daily flag
Flora appends a 2–3 sentence flag to `state/flags.md` for Aria: what happened today, one specific recommendation. If the run backfilled gap dates or wrote suspicious-day flags, she notes both.

---

## Output

- One or more `data/hot/nutrition/nutrition_app_daily_YYYY-MM-DD.md` files written or updated
- Zero or more suspicious-day flags appended to `state/flags.md`
- One daily summary flag appended to `state/flags.md` for Aria

---

## What Can Go Wrong

| Problem | Behavior |
|---------|----------|
| [NUTRITION_APP] email not yet in Gmail | Skips today silently; tomorrow's backfill picks it up |
| Gmail API unavailable | Flora should flag Aria with `priority: attention` and note the pipeline is down |
| Duplicate suspicious-day flag | Prevented by checking `state/flags.md` for existing open flags before writing |
| Manual-entry file gets overwritten by backfill | Prevented by `processed_by: Flora-manual-entry` check — backfill skips manual files |
