# Crico Scorer — Flutter App

A complete cricket scoring app rebuilt from scratch, with the **strike rotation bug fixed**.

## The Bug That Was Fixed

**Original bug:** When `numberOfBowler` was set to 2 (or more) in settings,
the over is split between multiple bowlers. After bowler 1 finished their
segment (e.g. 3 balls), the app incorrectly auto-swapped the striker whenever
runs were even (0, 2, 4, 6) — treating the bowler change like an end-of-over.

**Fix (see `lib/Helpers/scoring_engine.dart`):**
- Strike rotates **ball-by-ball** on odd bat runs (standard cricket)
- Strike rotates at **end of over** (6 legal balls complete) — always
- A **mid-over bowler segment change** (bowlerSegmentComplete) **NEVER** triggers an automatic strike change

## Features

- Create teams and players
- New match with configurable overs, bowlers per over, extra rules
- Live scoring: runs (0–6), wide, no ball, byes, leg byes, wickets
- Manual strike swap button
- Auto bowler selection after each over / bowler segment
- Auto batsman selection after wickets
- Scoreboard with batting, bowling, fall of wickets
- Match history
- Default settings

## Build Instructions

### Requirements
- Flutter SDK 3.x (`https://flutter.dev/docs/get-started/install`)
- Android SDK (API 21+)
- Java 11+

### Steps

```bash
# 1. Install dependencies
flutter pub get

# 2. Run on connected device or emulator
flutter run

# 3. Build release APK
flutter build apk --release
# Output: build/app/outputs/flutter-apk/app-release.apk

# 4. Build split APKs (smaller, recommended)
flutter build apk --split-per-abi
```

## Project Structure

```
lib/
├── main.dart                    # App entry point
├── Models/
│   └── models.dart              # All data models
├── Helpers/
│   ├── db_helper.dart           # SQLite database layer
│   └── scoring_engine.dart      # ★ Core scoring logic (bug fixed here)
├── Providers/
│   ├── score_board_provider.dart  # Live match state
│   └── providers.dart           # Team, Player, Match, History providers
└── Screens/
    ├── home_screen.dart         # Home, Teams, Players, History, Settings
    ├── update_score_screen.dart  # Live scoring UI
    ├── scoreboard_screen.dart   # Match scoreboard
    └── choose_batsman_screen.dart  # Batsman/Bowler selection
```

## Database Schema

- `teams` — team records
- `players` — player records linked to teams
- `matches` — match configuration and state
- `innings` — inning state (current striker, over, runs, wickets)
- `overs` — over records with bowler segment support
- `balls` — every delivery recorded
- `player_innings` — batting stats per inning
- `bowler_innings` — bowling stats per inning
- `fall_of_wickets` — wicket fall records
- `default_settings` — user preferences
