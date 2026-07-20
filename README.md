# CexiGEO — Geomancer Potency Calculator (CatsEyeXI, Lv.75)

An interactive potency/tier calculator for **Geomancer** on the **CatsEyeXI** private server (level 75 era). Set your Geomancy and Handbell skill and instantly see every Indi-/Geo- spell's value, the exact combined-skill breakpoints for the next tier, and where you hit diminishing returns.

**Live:** https://riuslol.github.io/CexiGEO/

It's a single self-contained `index.html` — no build step, no dependencies, no tracking. Open the file locally or use the link above.

---

## Features

- **Separate Geomancy / Handbell sliders** with a live **combined** total (Handbell only counts while a bell is equipped — toggle included).
- **Every spell, both models** — smooth `%` buffs/debuffs and stepped tick/point spells, computed the way the game actually does it.
- **Idris toggle** — adds its bonus correctly per spell (not a flat "+2").
- **Next tier** column — the exact combined skill for your next potency step and how far off you are.
- **Return / +100 skill** column — how much each spell gains per 100 skill invested, with a green→red efficiency bar so diminishing returns are obvious at a glance.
- **The maths** panel — the formulas are shown, nothing hidden.
- **CatsEyeXI gear list** — every confirmed Geomancy+, Geomancy skill+ and Handbell skill+ piece, with a one-click "max-skill build" button.

---

## How it works

Potency scales with your **combined skill** = Geomancy Skill + Handbell Skill (merits add +16 to each). Maximum potency is reached at **900 combined skill**.

- **Percentage spells** (Fury, Barrier, Haste, Slow, Paralyze, Frailty) scale smoothly:
  `value = Low + (High − Low) × combined ÷ 900`
- **Tick / point spells** (MP/HP/dmg per tick; accuracy, evasion, MAB, MDB, stat points) step in whole units:
  `tier = FLOOR(combined ÷ SkillPerTier)` → `value = Low + tier`, capped.
- **Idris** (Ergon **dagger**, Geomancy+2) adds a flat bonus of `2 × the spell's per-point Geomancy+ value` — e.g. Regen +4, Precision +10, Fury +5.4%. (Being a dagger, it does **not** grant Handbell skill.)

Endpoint data (Low / High / per-point values) comes from Square Enix's dev posts *Effect Values of Indicolure Enfeebling / Enhancement Spells* (Camate), mirrored on BG-Wiki's `Category:Geomancy`.

---

## Verified in-game

The model isn't guessed — every scaling type was confirmed against live readings on CatsEyeXI:

| Spell | Type | Reading | Matches |
|---|---|---|---|
| Fury | linear % | 330 → 420 attack @525 + Idris | ✅ |
| Barrier | linear % | 332 → ~450 defense @525 + Idris | ✅ |
| Refresh | step tick | 2/tick @241 naked · 5/tick @525 + Idris | ✅ |
| Regen | step tick | 31/tick @525 + Idris | ✅ |
| Precision | step point | 312 → 351 acc @525 + Idris | ✅ |
| CHR (stat) | step point | 67 → 86 @525 + Idris | ✅ |

---

## Two corrections vs the common tier chart

If you're comparing against the widely-shared BG-Wiki-style chart, note two CatsEyeXI-specific differences (both settled by the in-game tests above):

- **Refresh is 180 skill/tier, not 120.** At 120 the naked reading would be 3/tick and the geared one 6–7; at 180 they land on 2 and 5. (Retail FFXIclopedia also lists 180.)
- **Idris is not a flat "+2 tiers."** It's Geomancy+2, and each point adds the spell's per-point value — so it's only "+2" on spells where one tier equals one unit (like Refresh). Everywhere else it's larger.

---

## Max skill at Lv.75

`Geomancy 225 + 16 merit + 59 gear = 300` · `Handbell 225 + 16 + 39 = 280` → **combined 580**.

The in-app gear list is the confirmed set. AF+1 / Dynamis augment values aren't all in yet — if you know them, they're easy to add (see below).

---

## Editing / contributing

Everything lives in `index.html`. The spell data and gear lists are plain arrays near the bottom of the file:

```js
// name, effect, unit, Low, High, model, cap, skillPerTier, geoInc
["Fury","Attack","%",4.6,34.7,"lin",900,30,2.7],
...
// gear: name, slot, value
const GEAR_GEO=[["Bagua Tunic +1","Body",18], ...];
```

Add a gear piece by dropping a row into the relevant `GEAR_*` list; add or fix a spell by editing its array entry. No tooling required — save and refresh.

Corrections and additions welcome via issue or PR.

---

## Credits

- Mechanics data: Square Enix dev posts (Camate) and BG-Wiki `Category:Geomancy`.
- CatsEyeXI-specific values verified in-game by the CEXI Geomancer community.

*This is a fan-made tool for a private server and is not affiliated with Square Enix or CatsEyeXI.*
