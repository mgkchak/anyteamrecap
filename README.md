# Damascus Swim — Meet Recap Generator

Swim meet recap generator for the Damascus (DA) swim team. Paste the MCSL meet results page to instantly generate a weekly recap: Top 5 Most Improved, Top Point Scorers, All-Star Qualifiers, Personal Bests, and drop-time buckets by second. MCSL All-Star nominating standards built in. Export to clipboard or Markdown.

No server, no account, no API — open the file in any browser and it works.

---

## How to Use

### A-Meet: Paste Mode

1. Log in to [mcsl.org](https://mcsl.org) and navigate to your meet results page
   - URL format: `mcsl.org/display-results-dual/2025/week1/DA-WL`
2. Press **Ctrl+A** to select all, then **Ctrl+C** to copy
3. Open the app, confirm **A-Meet** is selected, and paste into the text box
4. Click **⚡ Generate Recap**

Seed times are embedded in the MCSL results page — no separate file needed.

### B-Meet: PDF Upload

1. Switch to the **📄 B-Meet PDF** tab
2. Select the **Home Team** and **Away Team** from the dropdowns
   - Damascus is pre-selected as the home team
3. Upload the HY-TEK Meet Manager **Meet Summary PDF** (the file containing seed and final times for all Damascus swimmers)
4. Click **⚡ Generate Recap**

---

## What Gets Generated

| Section | A-Meet | B-Meet |
|---|:---:|:---:|
| Meet header (teams + scores) | ✓ | ✓ (no scores) |
| Top 5 Most Improved | ✓ | ✓ |
| Top Point Scorers | ✓ | — |
| All-Star Qualifiers | ✓ | ✓ |
| Personal Bests | ✓ | ✓ |
| Drop Buckets (2s, 3s, etc.) | ✓ | ✓ |

### Header Format
- **A-Meet:** `Week 1 A-Meet: DAMASCUS (405.5) VS WESTLEIGH (379.5)`
- **B-Meet:** `Week 1 B-Meet: CLARKSBURG VILLAGE at DAMASCUS`

### Drop Rounding Rules
- Drop = `seed time − final time`
- Bucketed by **floor**: 2.00–2.99 = 2-second drop, 3.00–3.99 = 3-second drop, etc.
- Only individual drops ≥ 2 seconds appear in drop buckets
- Most Improved uses **combined** drop across all events (no per-event minimum), requires drops in **3 or more events**

---

## All-Star Qualifying Standards

Hardcoded from the official [MCSL nominating times](https://mcsl.org/all-stars-nominating-times/) table, split by age group, gender, and event.

| Age Group | Events |
|---|---|
| 8 & Under | 25M Free, 25M Back, 25M Breast, 25M Fly |
| 9–10 | 50M Free, 25M Back, 25M Breast, 25M Fly |
| 11–12 | 50M Free, 50M Back, 50M Breast, 50M Fly |
| 12 & Under | 100M IM |
| 13–14 | 50M Free, 50M Back, 50M Breast, 50M Fly, 100M IM |
| 15–18 | 100M Free, 100M Back, 100M Breast, 50M Fly, 100M IM |

---

## Exporting the Recap

- **Copy Text** — copies plain text to clipboard, ready to paste into an email
- **⬇ Download .md** — saves a Markdown file for any Markdown-compatible editor

---

## A-Meet Parsing Notes

The parser expects the standard MCSL results page format:

```
Event 1 - Male 12&U 100M Medley
Seed Time    Final Time    Points
1    Joung, Joshua S (12)(WL)    1:11.82    1:12.44    6
4    Zhao, Marc (10)(DA)         1:33.07    1:34.75    2
```

Key details:
- Swimmer names in `Last, First [MI] (age)(TEAM)` format → converted to `First Last`
- Team is the parenthetical at the end of the name: `(DA)`, `(WL)`, etc.
- Two time columns: seed first, final second
- Points as the last column
- `DQ`, `NS`, `SCR` entries are excluded from all calculations
- Half-point ties (e.g. `2.5`) are handled
- Relay events are automatically skipped

## B-Meet Parsing Notes

The parser expects the HY-TEK Meet Manager **Meet Summary** report:

```
1 Agusto-Cox, Katerina - Female - Age: 15 - ID#: 030811KATAAGUS
#5 Women 15-18 100 IM Finals 1:25.50 1:24.27 (5) *
#15 Women 15-18 100 Free Finals 1:16.07 1:15.96 (4) *
```

Key details:
- One swimmer per block; name in `Last, First [MI]` format → converted to `First Last`
- Gender and age parsed from the swimmer header line
- Seed and final times are the two values after `Finals`
- `NT` seed = no prior time (swimmer still counts for personal bests if they finish)
- `DQ`, `NS`, `SCR`, `EXH` finals are excluded
- Asterisk (`*`) denotes a drop per HY-TEK, but the app calculates drops independently

---

## Files

| File | Description |
|---|---|
| `swim-recap-generator.html` | The application — open in any browser |
| `README.md` | This file |
| `CHANGELOG.md` | Version history |
