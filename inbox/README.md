# inbox/

Drop zone for raw data exports. Agents check this on wake when told a file is here.

## How it works

1. Drop any data export here (fitness tracker export, nutrition app export, medical document, etc.)
2. Tell the relevant agent in their session that a file is in the inbox
3. The agent moves it to the correct `data/hot/[domain]/` location and renames it

## File naming after filing

`[source]_YYYY-MM-DD.[ext]`

Examples: `garmin_export_2026-04-17.csv`, `hevy_export_2026-04-17.csv`

Agents move and rename silently — no announcement unless something is wrong.
