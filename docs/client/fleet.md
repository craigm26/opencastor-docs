# Fleet View

The Fleet screen shows all registered robots with live status.

## Robot cards

Each card shows:

- Robot name + RRN
- Hardware tier badge (`pi5-hailo`, `pi5`, etc.)
- Online/offline status
- Active task (if any)
- Contribute status (idle / working / paused)

## Fleet Leaderboard

The **Leaderboard** tab shows harness research progress:

- Search space progress bar (`435 / 263,424 configs`)
- Current champion config with OHB-1 score
- Contributor badges (which robots evaluated the champion)
- "More research projects coming" placeholder for Q3 2026 BOINC integration

## Offline fallback

All fleet data is cached locally. If Firestore is unreachable, the app shows the last-known state with a warning chip — it never crashes on missing data.
