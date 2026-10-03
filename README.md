# Castaway live season config

`season.json` is read by the Castaway iPhone game at launch. Edit it, commit, push, and every phone
picks it up within a few minutes, no App Store update needed. The app keeps the last good copy and
ignores a broken file, so a typo can never break the game — but it also means a typo silently does nothing.

## What's in it
- `version` — bump it whenever you change the file (the title screen shows it, so you can tell a phone has it).
- `message` — a line shown on the title screen instead of the tagline, or `null`.
- `formats` — the seasons a player can pick: players, tribes (name, colour, emblem asset), merge size, jury size, finalists, minimum tribe size.
- `economy` — growth points per day, attribute cap, where points get expensive, Mystery card cost, hand size, plays per day.
- `curveballs` — twists with a chance per day, a day window and a cap. Kinds the app knows: `tribeSwap`, `nobodyGoesHome`. Unknown kinds are ignored.
- `roster` — the character pool. Each has an id, name, emoji, optional art asset, role, tagline, archetype
  (`Challenge Beast`, `Strategist`, `Loyal Soldier`, `Social Butterfly`, `Wildcard`, `Snake`, `Underdog`), personality
  (`strategy`, `loyalty`, 1–5), starting qualities (`strength`, `charm`, `cunning`, `grit`, 0–10) and what they grow (`spending`).

## Rules the app enforces before accepting a file
`schemaVersion` must match the app's, every format needs at least two tribes and enough characters, `mergeAt` must sit
between the finalist count and the player count, and the default format must exist. Check with:
```bash
python3 -c "import json; json.load(open('season.json'))"
```
The full validator is `SeasonConfig.problem()` in the game's source. The starting file is generated from the game's
built-in season with `build/sim export-config`.
