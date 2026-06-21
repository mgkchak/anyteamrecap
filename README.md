# MCSL Meet Recap Generator

Swim meet recap generator for MCSL dual meets. Paste A-meet results or upload a B-meet PDF to instantly generate a formatted weekly recap: Top 5 Most Improved, Top Point Scorers, All-Star Qualifiers, Personal Bests, and drop-time buckets by second. MCSL All-Star nominating standards built in. Export to clipboard or Markdown.

No server, no account, no API — open the file in any browser and it works.

---

## How to Use

### A-Meet (Paste Results)

1. Visit **mcsl.org** and select **Meet Results** from the left-hand menu, then select the year and week
2. Scroll down to the meet you want and click to open it
3. Select everything from **20XX Week [#] Dual Meet [Team] at [Team]** at the top all the way through the last result at the bottom of the page
4. Copy (**Ctrl+C**), then paste into the box (**Ctrl+V**)
5. Click **⚡ Generate Recap**

### B-Meet (Upload PDF)

1. Switch to the **📄 B-Meet (Upload PDF)** tab
2. Select the **Home Team** and **Away Team** from the dropdowns
3. Upload the HY-TEK Meet Manager **Meet Summary PDF**
4. Click **⚡ Generate Recap**

---

## What Gets Generated

| Section | A-Meet | B-Meet |
|---|:---:|:---:|
| Meet header (teams + scores) | ✓ | ✓ (no scores) |
| Top 5 Most Improved (3+ Events) | ✓ | ✓ |
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
- **Most Improved** requires drops in **3 or more events**; ranked by combined total drop; top 5 shown
- **Biggest single-event drop** highlights the largest drop in any one event

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
- `DQ`, `NS`, `SCR` entries excluded from all calculations
- Half-point ties (e.g. `2.5`) handled correctly
- Relay events automatically skipped

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
- `NT` seed = no prior time (excluded from drops but counts toward personal bests if finished)
- `DQ`, `NS`, `SCR`, `EXH` finals excluded entirely
- Asterisk (`*`) denotes a drop per HY-TEK; the app calculates drops independently

---

## Files

| File | Description |
|---|---|
| `swim-recap-generator.html` | The application — open in any browser |
| `README.md` | This file |
| `CHANGELOG.md` | Version history |

---

*Created by Melanie Kurimchak in collaboration with Claude Sonnet 4.6 (Anthropic). Melanie conceived the tool, defined all requirements, tested functionality, and directed development through iterative feedback.*
