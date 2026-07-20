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

