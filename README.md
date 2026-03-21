# GW2 Alternate Build Optimization

A browser-based tool for Guild Wars 2 players to compare gear prefix combinations against a custom target build, analyse per-stat deviations, and automatically find the optimal prefixes for free equipment slots.

---

## Features

- **Full gear coverage** — configure all 16 slots across Armor (6), Trinkets (6), and Weapons (4) independently
- **Per-slot rarity** — toggle each individual slot between Ascended and Exotic
- **Per-slot weapon hand** — toggle each weapon slot between 1H (one-handed) and 2H (two-handed) stat values
- **Current vs Target build** — set prefixes for your current equipped gear and the ideal build you are working toward, side by side
- **Per-slot lock toggles** — lock any slot to fix it as a known piece; locked slots count toward totals but are excluded from optimization
- **Deviation scoring** — a live score shows how far your current build is from the target, broken down per stat with a colour-coded progress bar and grade label
- **Acceptable deviation ranges** — set per-stat tolerance bands as percentages (e.g. Power can be up to 5% below target, Ferocity up to 10% above) to filter optimization results to combinations that suit your playstyle
- **Find Best combinations** — automatically computes the top 10 prefix combinations for your free slots that minimise deviation from the target, filtered by your tolerance constraints
  - Exact brute-force for up to 4 free slots
  - Hill-climbing with restarts for 5+ free slots (near-optimal)
- **No install required** — single self-contained HTML file, runs entirely in the browser with no server, no dependencies, no data sent anywhere

---

## Usage

1. Open `GW2-Alternate-Build-Optimization.html` in any modern browser — no web server needed, just double-click the file
2. **Current Build** (left column): for each slot, set the **Asc/Exo** rarity toggle, the **1H/2H** hand toggle (weapons only), and select a prefix. Toggle the lock switch to mark a slot as fixed — locked slots are counted in totals but skipped by the optimizer
3. **Target Build** (middle column): select the prefix you want for each slot. Use the "set all to…" dropdown to quickly fill all slots with a single prefix, then adjust individual pieces as needed
4. **Analysis** (right column): review the deviation score and per-stat breakdown. Optionally enter acceptable deviation ranges, then click **Compute optimal for free slots** to find the best prefix combinations

---

## Supported Prefixes

All standard ascended armor, weapon, and trinket prefixes from the base game and expansions are included. Prefixes with slot-type restrictions are automatically hidden from ineligible slots:

| Prefix | Restriction |
|--------|-------------|
| Forsaken | Weapons only |
| Seraph | Not available on Back Item |

Full prefix list (alphabetical):

> Apothecary's · Assassin's · Berserker's · Bringer's · Carrion · Cavalier's · Celestial · Cleric's · Commander's · Crusader · Demolisher · Dire · Diviner's · Dragon's · Forsaken · Giver's · Grieving · Harrier's · Knight's · Magi's · Marauder · Marshal's · Minstrel's · Nomad's · Plaguedoctor's · Rabid · Rampager's · Ritualist's · Seraph · Sentinel's · Settler's · Shaman's · Sinister · Soldier's · Trailblazer's · Valkyrie · Vigilant · Viper's · Wanderer's · Zealot's

---

## Stat Values Reference

All values are sourced from the [GW2 Wiki — Attribute combinations](https://wiki.guildwars2.com/wiki/Attribute_combinations) for **level 80** equipment.

### Armor

| Slot | Asc 3-stat maj | Asc 3-stat min | Asc 4-stat maj | Asc 4-stat min | Asc Cel | Exo 3-stat maj | Exo 3-stat min | Exo 4-stat maj | Exo 4-stat min | Exo Cel |
|------|---------------|---------------|---------------|---------------|---------|---------------|---------------|---------------|---------------|---------|
| Chest | 141 | 101 | 121 | 67 | 67 | 134 | 96 | 115 | 63 | 63 |
| Legs | 94 | 67 | 81 | 44 | 44 | 90 | 64 | 77 | 42 | 42 |
| Head | 63 | 45 | 54 | 30 | 30 | 60 | 43 | 51 | 28 | 28 |
| Shoulders | 47 | 34 | 40 | 22 | 22 | 45 | 32 | 38 | 21 | 21 |
| Gloves | 47 | 34 | 40 | 22 | 22 | 45 | 32 | 38 | 21 | 21 |
| Boots | 47 | 34 | 40 | 22 | 22 | 45 | 32 | 38 | 21 | 21 |

### Trinkets

| Slot | Asc 3-stat maj | Asc 3-stat min | Asc 4-stat maj | Asc 4-stat min | Asc Cel | Exo 3-stat maj | Exo 3-stat min | Exo 4-stat maj | Exo 4-stat min | Exo Cel |
|------|---------------|---------------|---------------|---------------|---------|---------------|---------------|---------------|---------------|---------|
| Amulet | 157 | 108 | 133 | 71 | 72 | 120 | 85 | 102 | 56 | 56 |
| Ring | 126 | 85 | 106 | 56 | 57 | 90 | 64 | 77 | 42 | 42 |
| Accessory | 110 | 74 | 92 | 49 | 50 | 75 | 53 | 64 | 35 | 35 |
| Back Item | 63 | 40 | 52 | 27 | 28 | 30 | 21 | 26 | 14 | 14 |

> **Note:** Exotic trinket values are the base piece only — jewel upgrade slot contributions are not included.

### Weapons

| Type | Asc 3-stat maj | Asc 3-stat min | Asc 4-stat maj | Asc 4-stat min | Asc Cel | Exo 3-stat maj | Exo 3-stat min | Exo 4-stat maj | Exo 4-stat min | Exo Cel |
|------|---------------|---------------|---------------|---------------|---------|---------------|---------------|---------------|---------------|---------|
| 1H | 125 | 90 | 108 | 59 | 59 | 120 | 85 | 102 | 56 | 56 |
| 2H | 251 | 179 | 215 | 118 | 118 | 239 | 171 | 205 | 113 | 113 |

---

## Deviation Tolerance System

The **Acceptable Deviation Ranges** panel lets you constrain what the optimizer considers a valid result. Each stat in your target build gets two optional inputs:

| Field | Meaning | Example |
|-------|---------|---------|
| **Min** | How far *below* target is acceptable, as a percentage | `-5` = up to 5% under target is ok |
| **Max** | How far *above* target is acceptable, as a percentage | `+10` = up to 10% over target is ok |

Leave either field blank to apply no constraint in that direction. A green dot next to each stat confirms your current build satisfies the constraint; red means it does not. Find Best only returns combinations that pass all active constraints.

---

## optimization Algorithm

| Free slots | Method |
|-----------|--------|
| 1–3 | Exact brute force |
| 4 | Exact brute force (~1.5M combinations) |
| 5+ | Hill-climbing with 80 random restarts + one start per prefix (near-optimal) |

When tolerance constraints are active, the hill-climbing algorithm uses a penalty function to steer toward feasible regions of the search space, then filters final results to only include combinations that satisfy all constraints.

---

## Browser Compatibility

Tested in Chrome, Firefox, Edge, and Safari. Requires a browser with ES2020+ support (any modern browser released after 2020). No internet connection required after initial load of Google Fonts.

---

## License & Disclaimer

Copyright (c) 2026 JustAChance. All rights reserved.

This tool may be used for personal, non-commercial purposes only. Redistribution, modification, or use in any commercial product or service is strictly prohibited without the express written consent of the author.

This tool is provided "as is", without warranty of any kind, express or implied. The author shall not be held liable for any direct, indirect, incidental, or consequential damages arising from the use or misuse of this tool, including but not limited to any illicit, unlawful, or unauthorised use by third parties. Use of this tool constitutes acceptance of these terms.

---

## Acknowledgements

- Stat values sourced from the [Guild Wars 2 Wiki](https://wiki.guildwars2.com/wiki/Attribute_combinations)
- Guild Wars 2 is a registered trademark of ArenaNet, LLC. This tool is an unofficial fan project and is not affiliated with or endorsed by ArenaNet or NCSoft
