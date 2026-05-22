# MCSL Swim Meet Recap Generator

MCSL swim meet recap generator for any team. Paste any dual meet results page, select your team, and generate a formatted weekly recap: Top 5 Most Improved, Top Point Scorers, All-Star Qualifiers, Personal Bests, and drop-time buckets by second. MCSL All-Star nominating standards built in. Export to clipboard or Markdown.

No server, no account, no API — open the file in any browser and it works.

---

## How to Use

### Paste Mode (recommended)

1. Log in to [mcsl.org](https://mcsl.org) and navigate to your meet results page
   - URL format: `mcsl.org/display-results-dual/2025/week1/DA-WL`
2. Press **Ctrl+A** to select all, then **Ctrl+C** to copy
3. Open the app, select **A-Meet** or **B-Meet**, and paste into the text box
4. A **team selector** will appear — choose which team to generate the recap for
5. Click **⚡ Generate Recap**

Seed times are embedded in the MCSL results page, so no separate file is needed.

### PDF Mode

If you have printed PDFs instead of a URL:

1. Switch to the **Upload PDFs** tab
2. Upload the **Meet Results PDF** (official results sheet)
3. Upload the **Drops / Seed Times PDF** (pre-meet seed times for your team)
4. Click **⚡ Generate Recap**

---

## What Gets Generated

| Section | Details |
|---|---|
| **Header** | Meet name, week number, A/B designation, both team scores |
| **Top 5 Most Improved** | Swimmers who dropped time in 3+ events, ranked by combined total drop |
| **Top Point Scorers** | Highest point totals by gender; handles ties; shows first-place finish count |
| **All-Star Qualifiers** | Swimmers who hit MCSL nominating times, checked against official standards by age group, gender, and event |
| **Personal Bests** | Total count of time drops across the team, plus the single biggest drop |
| **Drop Buckets** | Swimmers grouped by floor-second drop (2-second, 3-second, etc.) — only drops ≥ 2 seconds are listed |

### Drop Rounding Rules
- Drop time is calculated as `seed time − final time`
- Bucketed by **floor**: 2.00–2.99 = 2-second drop, 3.00–3.99 = 3-second drop, etc.
- Only individual drops ≥ 2 seconds are listed in the buckets
- Most Improved uses the **combined** drop across all events (no minimum per event)

---

## All-Star Qualifying Standards

Standards are hardcoded from the official [MCSL nominating times](https://mcsl.org/all-stars-nominating-times/) table, split by age group, gender, and event.

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

- **Copy Text** — copies the plain text to your clipboard, ready to paste into an email
- **⬇ Download .md** — saves a Markdown file for any Markdown-compatible editor or email client

---

## Parsing Notes

The parser is built around the MCSL results page format:

```
Event 1 - Male 12&U 100M Medley
Seed Time    Final Time    Points
1    Joung, Joshua S (12)(WL)    1:11.82    1:12.44    6
4    Zhao, Marc (10)(DA)         1:33.07    1:34.75    2
```

Key format details it handles:
- Swimmer names in `Last, First [MI] (age)(TEAM)` format → converted to `First Last`
- Team abbreviation embedded in the name as `(DA)`, `(WL)`, etc.
- Two time columns: seed first, final second
- Points as the last column (plain integer)
- `DQ`, `NS`, `SCR` entries (excluded from drops/scoring)
- Half-point ties (e.g. `2.5` points)
- Relay events are automatically skipped

If parsing produces unexpected results, click **"show parsing debug info"** below the output to inspect what the parser detected.

---

## Files

| File | Description |
|---|---|
| `swim-recap-generator.html` | The application — open this in any browser |
| `README.md` | This file |
