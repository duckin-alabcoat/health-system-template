# Flow: Flora Manual Entry

Used when the user needs to provide nutrition data for a day that [NUTRITION_APP] logged incorrectly or not at all. Runs as an interactive Flora session. See ADR-007 for design rationale.

---

## Trigger

One of:
- the user opens a Flora session directly to correct a specific day
- Aria surfaces a suspicious-day flag at check-in and the user says he wants to provide the actual numbers (not just confirm the day was accurate)

---

## Steps

### 1. Wake
Flora reads her outbound file, then her knowledge base.

### 2. the user provides data
In any format:
- **Conversation:** "I had Greek yogurt for breakfast, a Chick-fil-A sandwich for lunch, and [PARTNER_NAME] made chicken and rice for dinner"
- **Screenshot:** the user pastes or uploads a [NUTRITION_APP] screenshot; Flora reads it directly
- **Description/photo:** the user describes or photographs a meal; Flora estimates

### 3. Flora parses
Flora derives the nutritional values. All nutrition data is estimate-stacked at every layer — Flora's estimate is no less trustworthy than [NUTRITION_APP]'s. She does not chase false precision.

### 4. Readback — no silent writes
Flora reads back the proposed diff before writing anything:

```
Here's what I'd write for April 14:
  calories: 1,840 (was 680)
  protein: 128g (was 42g)
  fiber: 18g (was 6g)
  fat: 62g (was 24g)
  sodium: unchanged at 2,100mg
  weight: unchanged at 192.4 lbs
```

the user confirms or corrects. Flora writes only after explicit confirmation.

### 5. Write with provenance
On confirmation, Flora writes the updated data file with additional frontmatter:

```yaml
processed_by: Flora-manual-entry
corrected_from: Flora          # or "architect-backfill" etc.
corrected_at: 2026-04-16 14:30
source_format: conversation    # or "screenshot" or "mixed"
```

Partial corrections merge — fields the user provided are updated, others stay at their current values.

### 6. Flag management
If the day had an open suspicious-day flag in `state/flags.md`, Flora closes it:

```yaml
status: closed
closed_by: Flora
closed_date: YYYY-MM-DD
close_note: Manual entry provided by the user in interactive session.
```

### 7. Session log
Flora writes a session log. Sets `aria_flag: true`. The `## FOR ARIA` section names the date corrected and summarizes what changed.

---

## Confirm-As-Is Path (Lightweight — No Flora Session Needed)

If the user tells Aria at check-in that a flagged day was accurate as logged ("April 14 was a fasting day, that's right"), Aria handles it directly:

1. Aria writes `state/outbound/flora.md` with a confirm-as-is entry
2. On Flora's next scheduled wake (8:15am daily or 8:30am Monday), Flora reads the outbound file
3. Flora stamps the data file: `user_confirmed_as_is: true`, `confirmed_at`, `confirmed_source: aria-relay`
4. Flora closes the open flag for that date
5. Flora deletes the outbound file

This path does not require an interactive Flora session.

---

## Source Hierarchy

Manual entries always win over [NUTRITION_APP]. If the Monday weekly reconciliation finds that [NUTRITION_APP]'s weekly report differs from a manual-entry file by more than 50 calories or 5g protein, Flora keeps the manual number and writes a conflict flag for Aria. Aria decides whether to surface it.

---

## What Can Go Wrong

| Problem | Behavior |
|---------|----------|
| the user confirms wrong data | Flora wrote what was confirmed; the data file has the correction provenance. Open a new Flora session to correct again — each correction stamps the prior `processed_by` as `corrected_from`. |
| Confirm-as-is outbound file sits unread | Prim flags outbound files unread for 3+ days. Aria will see the flag on her next nightly. |
| Session ends before confirmation | Flora's topic-break write preserves the conversation to the session log. The flag stays open. Next daily run re-checks and leaves it in place. |
