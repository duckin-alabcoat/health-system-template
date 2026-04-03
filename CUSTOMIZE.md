<!-- TEMPLATE: This document guides you through every placeholder and what to fill in. -->

# Customization Guide — Fill in Your Details Here

Every [PLACEHOLDER] in the agent files and bootstrap.md needs to be replaced with your personal information. This guide lists every placeholder and what it means.

## Personal Profile

### [USER]
Your name. Used throughout all agent prompts.

**Examples:** "Sarah", "James", "Alex"

### [AGE]-year-old [SEX]
Your age and sex/gender, used in context where age-appropriate coaching matters.

**Examples:** "42-year-old female", "35-year-old non-binary", "63-year-old male"

**Note:** Brynn's coaching changes based on age. If you're 35, her advice will be different from a 63-year-old. Update this accurately.

### [LOCATION]
Where you live. Used for context like climate, local resources, and time zone.

**Examples:** "Portland, Oregon", "Chicago, Illinois", "London, UK"

## Health & Medical

### [PCP] (Primary Care Physician)
Your primary care doctor's name. Used in Vera's pre-appointment preparation and post-visit debriefs.

**Example:** "Dr. Sarah Johnson"

### [ENDOCRINOLOGIST]
Your endocrinologist, if you have one. If not, remove this reference.

**Example:** "Dr. Marcus Chen"

### [CARDIOLOGIST]
Your cardiologist, if you have one. If not, remove this reference.

**Example:** "Dr. Elena Rodriguez"

### [CONDITION_1], [CONDITION_2], etc.
Your diagnosed medical conditions. Replace these throughout the agent files with your actual conditions.

**Examples:** "asthma", "type 2 diabetes", "lower back pain", "GERD", "anxiety"

**Note:** In the bootstrap.md file, list all conditions. In individual agent files, customize the context to how each condition affects that agent's domain. For example, Vera needs to know all conditions; Flora only cares about conditions affecting nutrition (diabetes, GERD, etc.).

### [WEIGHT_LOSS_MEDICATION]
If you're on a weight loss medication, replace this with the actual medication name. If not, remove all references to this.

**Examples:** "Ozempic (semaglutide)", "Saxenda (liraglutide)", "Contrave (naltrexone/bupropion)"

**Note:** Preserve the description of what the medication does and its effects (appetite suppression, muscle loss risk, etc.). The drug name is just the [PLACEHOLDER].

### [CURRENT_WEIGHT]
Your current weight.

**Example:** "210 lbs"

### [TARGET_WEIGHT]
Your goal weight.

**Example:** "180 lbs"

### [PEAK_WEIGHT]
Your highest historical weight, if relevant to longevity or health context.

**Example:** "275 lbs"

### [CUSTOMIZE: add baseline measurements]
In any agent that mentions specific health measurements (DEXA results, lab values, blood pressure readings, cholesterol numbers), replace with:
- The actual values if you have them
- Or note "[CUSTOMIZE: add your [specific test] baseline from [date]]" if you need to fill this in later

**Example:** "DEXA baseline ([DATE]): [your numbers]"

## Fitness & Training

### [HALF_MARATHON_PACE]
Your typical half marathon pace (or whatever your target race pace is).

**Example:** "8:45-9:00 per mile"

### [FITNESS_TRACKER]
Your primary fitness tracking device.

**Examples:** "Garmin", "Apple Watch", "Fitbit", "Oura Ring"

### [SMARTWATCH]
Your smartwatch, if different from your fitness tracker.

**Examples:** "Apple Watch", "Garmin Epix", "Wear OS watch"

### [RUNNING_APP]
Your running tracking app.

**Examples:** "Strava", "Garmin Connect", "Nike Run Club"

### [GYM_TRACKING_APP]
Your gym/strength training logging app.

**Examples:** "Hevy", "Strong", "JEFIT", "Fitbod"

### [HOME_EXERCISE_EQUIPMENT]
Home equipment you own or use.

**Examples:** "Peloton bike", "rowing machine", "resistance bands and dumbbells", "treadmill"

## Nutrition

### [FOOD_TRACKING_APP]
Your food logging application, if you use one. If not, remove references or note "manual logging".

**Examples:** "Lose It", "MyFitnessPal", "Cronometer", "MacroFactor"

### [GROCERY_STORE]
Your primary or local grocery store. Used for Flora's recommendations.

**Examples:** "Whole Foods", "Trader Joe's", "Kroger", "local co-op"

## Technology & Access

### [CALENDAR_EMAIL]
The Google Calendar email you share with Claude. This is your primary calendar for the system.

**Example:** "yourname.calendar@gmail.com" or "family-calendar@gmail.com"

### [USER_EMAIL]
Your primary Gmail address, used for receiving reports and for the system to process emails.

**Example:** "yourname@gmail.com"

### [PRIMARY_COMPUTER]
Your main computer where scheduled tasks run.

**Examples:** "Mac Studio", "MacBook Pro", "iMac", "Linux desktop"

### [SECONDARY_COMPUTER]
Your laptop or secondary device, if relevant.

**Examples:** "MacBook Air", "iPad Pro", "Windows laptop"

### [CLOUD_STORAGE]
The cloud storage service where you keep the Health System folder.

**Examples:** "iCloud Drive", "Google Drive", "Dropbox", "OneDrive"

### [NOTES_APP]
Your note-taking application. Used for Vera's appointment prep notes.

**Examples:** "Apple Notes", "Notion", "Obsidian", "Keep"

## Personal Context

### [PARTNER]
Your spouse or romantic partner's name, if they're relevant to your health system (especially for nutrition and sexual health context).

**Examples:** "Jordan", "Sam", "Taylor"

**Note:** If you live alone or prefer not to include partner context, remove these references entirely.

### [PREVIOUS_EMPLOYER]
Your former employer or career context, if relevant to mental health or retirement transitions.

**Examples:** "a Fortune 500 tech company", "your law firm", "the military"

**Note:** Only include if your mental health agent (Soleil) should be aware of career transitions or identity shifts.

### [LEARNING_INTERESTS]
Your hobbies, learning interests, or ways you spend non-working time.

**Examples:** "painting, gardening, and classical music", "woodworking and hiking", "learning piano and amateur radio"

**Note:** Used by Vita (longevity) and Soleil (mental health) as indicators of cognitive engagement and purpose.

## Medical & Health Targets

### Protein Target
Replace generic "[CUSTOMIZE: protein targets]" with your actual target.

**Calculation:** Generally 0.7-1.0g per pound of body weight for adults doing strength training.

**Example:** "160g protein per day" (for a 210-lb person doing strength training)

### Fiber Target
Replace generic "[CUSTOMIZE: fiber targets]" with your actual target.

**Recommended:** 25-35g per day for most adults.

**Example:** "28-35g fiber per day"

### Caloric Baseline
Your basal metabolic rate (BMR) or resting metabolic rate (RMR), if you have it.

**Example:** "1,750 cal/day RMR (from DEXA measurement)"

### Sleep Target
Your target sleep duration.

**Common range:** 6.5-8.5 hours per night.

**Example:** "7.5 hours per night"

### Race Calendar
Replace placeholder race dates with your actual schedule.

**Example:** "April 20 (half marathon), June 2 (marathon), October 15 (half marathon)"

## Replacing All Instances

Use find-and-replace in your text editor to handle these systematically:

1. **[USER]** → Your name (search and replace across all files)
2. **[LOCATION]** → Your location
3. **[AGE]-year-old [SEX]** → Your age/sex demographic
4. **[TARGET_WEIGHT]** → Your goal weight
5. **[CURRENT_WEIGHT]** → Your current weight
6. **[WEIGHT_LOSS_MEDICATION]** → Your medication (if applicable)
7. Continue with medical team, devices, apps, calendar, etc.

## Sections to Customize Beyond Placeholders

Some sections require more than placeholder replacement. They need context updates:

### Bootstrap.md — THE AGENTS Table
Update the status and notes for your actual agent situation. If you're missing agents, note it. If all 12 are complete, mark them as such.

### Flora.md — REAL-WORLD CONSTRAINTS
Update this to reflect your actual life:
- Who plans your meals? (Your partner? A meal prep service? You?)
- Where do you eat out? (Do these replace the examples)
- What's your actual level of control over food choices?

### Vera.md — APPOINTMENT CYCLE
Update the frequency of your appointments if different:
- PCP: 2x/year or 4x/year?
- Specialists: Once per year or quarterly?

### Brynn.md — TRAINING ENVIRONMENT
Update to match your actual gym/training setup:
- Do you have a personal trainer? If so, what's their role?
- What equipment do you actually have access to?
- What are your actual training days?

### Soleil.md — WHAT SOLEIL ASKS ABOUT
Update the examples of topics Soleil might explore:
- Update career/life transition context to match yours.
- Update learning interests to yours.
- Update relationship context to your actual relationships.

### Lumi.md — CONTEXT YOU CARRY
Update the personal context:
- If you're an ultramarathoner, that's different training than a half-marathon runner
- Update climate context (desert heat vs. Pacific Northwest rain)
- Age-appropriate grooming needs change if you're 35 vs. 65

### Lyra.md — THE GOAL
Update the target weight and current weight, obviously. But also:
- Is weight loss your goal? (Or muscle gain, or maintenance?)
- What short-term milestones make sense for you?

### River.md — HYDRATION
Update climate and environment context:
- If you live somewhere cold, hydration needs are different
- If you live at elevation, hydration needs are different
- Update to match your actual climate

### Vita.md — WHAT VITA WATCHES
Update the longevity concerns:
- Bone density might not be your primary concern
- Update to your actual health risks
- Update longevity goals (running at 70? hiking at 80? Just staying independent?)

## A Checklist for Customization

Go through each agent file systematically:

- [ ] **bootstrap.md** — All personal info, medical team, calendar, email, goals updated
- [ ] **brynn.md** — Age context, gym setup, training days, medical conditions updated
- [ ] **flora.md** — Weight goal, nutrition targets, meal planning context, app names updated
- [ ] **luna.md** — Sleep target, device names, any health context updated
- [ ] **vera.md** — Doctor names, appointment frequencies, medical conditions updated
- [ ] **soleil.md** — Career/life context, relationships, learning interests updated
- [ ] **lumi.md** — Age/sex, climate, personal care context updated
- [ ] **seren.md** — Medication list, health conditions, relationship context updated
- [ ] **vita.md** — Age, health concerns, longevity goals updated
- [ ] **river.md** — Climate, training context, health conditions updated
- [ ] **lyra.md** — Current weight, target weight, goals updated
- [ ] **pace.md** (when found) — Race calendar, training level, injuries updated
- [ ] **aria.md** (when found) — Calendar, email, agent names confirmed

## Tips for Successful Customization

1. **Do it completely** — Don't leave placeholders. The system works best when fully customized to your life.

2. **Be specific** — Generic customization (like "some weight loss goal") will be less effective than specific (like "reach 180 lbs by September").

3. **Update quarterly** — Your life changes. Update the system. Major life changes should trigger agent re-tuning.

4. **Preserve personality** — When customizing, keep the agent personalities intact. Brynn should still be blunt. Flora should still love food.

5. **Medical accuracy** — Get your actual conditions and medications right. This directly impacts recommendations.

6. **Be honest about constraints** — If you can't control restaurant choices, say so. If you have limited gym access, say so. The system adapts.

7. **Start simple** — If you're not sure about a customization, start basic and expand. You can always add more detail later.

---

**Next:** Read **bootstrap.md** to understand the full system, then begin filling in your agent files.
