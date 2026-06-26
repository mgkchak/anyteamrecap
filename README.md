# MCSL Meet Recap Generator

Swim meet recap generator for any MCSL team. Paste A-meet results or upload a B-meet PDF to instantly generate a formatted weekly recap: Top 5 Most Improved, Top Point Scorers, All-Star Qualifiers, Personal Bests, and drop-time buckets by second. MCSL All-Star nominating standards built in. Export to clipboard or Markdown.

No server, no account, no API — open the file in any browser and it works.

---

## How to Use

### A-Meet (Paste Results)

1. Visit **mcsl.org**, select **Meet Results** from the left-hand menu, then select the year and week
2. Scroll down to the meet you want and click to open it
3. Select everything from **20XX Week [#] Dual Meet [Team] at [Team]** at the top all the way through the last result at the bottom of the page
4. Copy (**Ctrl+C**), then paste into the box (**Ctrl+V**)
5. Select the **Home Team**, **Away Team**, and **Generate recap for** from the dropdowns that appear
6. Click **⚡ Generate Recap**

### B-Meet (Upload PDF)

1. Switch to the **📄 B-Meet (Upload PDF)** tab
2. Upload the meet recap PDF — teams are detected automatically
3. Select the **Home Team**, **Away Team**, and **Generate recap for** from the dropdowns that appear
4. Click **⚡ Generate Recap**

Supports both HY-TEK Meet Manager **Summary** format (one team, swimmer-first) and **Results** format (both teams, event-first).

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
1    Smith, John A (12)(AB)  1:11.82    1:12.44    6
4    Lee, Sam C (10)(AB)     1:33.07    1:34.75    2
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

Two HY-TEK PDF formats are supported and auto-detected on upload:

**Summary format** (swimmer-first):
```
1 Smith, John - Male - Age: 15 - ID#: XXXXXXXXXX
#5 Men 15-18 100 IM Finals 1:25.50 1:24.27 (5) *
```

**Results format** (event-first, both teams):
```
Event 1 Boys 12 & Under 100 SC Meter IM
Name Age Team Seed Time Finals Time
1 Smith, John 15 Home Swim Team 1:29.45 1:28.56
```

Key details for both:
- `NT` seed = no prior time (excluded from drops, still counts as personal best if finished)
- `DQ`, `NS`, `SCR`, `EXH` finals excluded entirely
- PDF ligature characters (`ﬁ`, `ﬂ`, `ϐ`) normalized automatically
- Team names in Results format (e.g. `Flower Valley Frogs`) resolved to MCSL abbreviations

---

## Troubleshooting

If output looks wrong, click **"show parsing debug info"** below the output. It shows detected teams, event count, entry count, and sample parsed entries — useful for identifying format mismatches.

---

## Files

| File | Description |
|---|---|
| `swim-recap-generator.html` | The application — open in any browser |
| `README.md` | This file |
| `CHANGELOG.md` | Version history |

---

*Created by Melanie Kurimchak in collaboration with Claude Sonnet 4.6 (Anthropic). Melanie conceived the tool, defined all requirements, tested functionality, and directed development through iterative feedback.*
