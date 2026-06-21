# Changelog

All notable changes to the MCSL Meet Recap Generator.

---

## [2026-06-21] — UI Polish & Cleanup

### Changed
- **App title & branding** — renamed to "MCSL Meet Recap Generator"; tagline updated to "Montgomery County Swim League · MCSL"; badge updated to swimmer emoji 🏊
- **Tab labels** — paste tab renamed to "📋 A-Meet (Paste Results)"; PDF tab renamed to "📄 B-Meet (Upload PDF)"
- **Paste instructions** — updated to reflect exact mcsl.org navigation steps; removed unnecessary seed time note
- **Most Improved heading** — "Top 5 Most Improved" → "Top 5 Most Improved (3+ Events)"
- **Biggest drop label** — "Biggest drop:" → "Biggest single-event drop:"
- **Meet title** — home team name now resolved dynamically from MCSL team list; no longer hardcoded
- **B-meet team dropdowns** — removed Damascus pre-selection; both dropdowns start at blank prompt
- **B-meet hint text** — removed reference to "Damascus swimmers"; now team-agnostic

### Removed
- Meet type A/B radio buttons (paste mode is always A-meet)
- `parseDropsPDF` and `lookupSeed` functions (dead code from earlier dual-PDF approach)
- `dropsMap` variable and all related threading through recap builder
- Hardcoded "DAMASCUS" from title output

### Added
- AI attribution footer

---

## [2026-06-18] — B-Meet Support

### Added
- **B-Meet PDF upload mode** — new tab for uploading HY-TEK Meet Manager summary PDFs from B-meets
  - Home and away team dropdowns pre-populated with all 90+ MCSL teams
  - Damascus pre-selected as home team by default
  - Parses swimmer blocks from HY-TEK format: `Last, First - Gender - Age: N` headers with `#N Event Finals SeedTime FinalTime (Place)` event lines
  - Age-to-age-group mapping (6&U, 8&U, 9-10, 11-12, 13-14, 15-18) derived from swimmer header
  - Handles `NT` seed times, `DQ`/`NS`/`SCR`/`EXH` finals, and middle initial / duplicate last name artifacts in names
- **B-Meet header format** — `Week 1 B-Meet: CLARKSBURG VILLAGE at DAMASCUS` (no scores, away at home)
- **Top Point Scorers suppressed for B-meets** — exhibition meets don't score; section is omitted from output

### Fixed
- JavaScript syntax errors caused by double-escaped backticks in template literals during code generation
- `flipName` now strips duplicate last-name words (e.g. `Fensterseifer Fideli, Benjamin Fideli` → `Benjamin Fensterseifer Fideli`)

---

## [2025-06-xx] — 2025 Season

### Added
- **Most Improved filter** — now requires drops in 3 or more events (previously any number of drops)
- **"Top 5 Most Improved" label** — section renamed from "Most Improved" to clarify the top-5 cutoff
- **A/B meet type selector** — radio toggle to specify meet type when pasting results; used in title formatting if not detected from page title
- **Week N A/B-Meet title format** — output header now reads `Week 1 A-Meet: DAMASCUS (405.5) VS WESTLEIGH (379.5)`

### Fixed
- **`\r\n` line endings** — browser copy-paste on Windows produced CRLF line endings that broke regex matching; now normalized before parsing
- **Points parsing cap removed** — previous `≤ 7` cap on points tokens incorrectly filtered out high point totals; now uses last plain integer token on each entry line
- **"each" wording for top scorers** — "each" now only appended when multiple swimmers are tied for the top score

### Initial features
- **Paste mode** — Ctrl+A / Ctrl+C from MCSL results page; seed times parsed inline (no separate file)
- **MCSL result format parser** — handles `Last, First [MI] (age)(TEAM)` name format, two time columns (seed + final), points column, DQ/NS/SCR entries, half-point ties, relay event skipping
- **Top 5 Most Improved** — combined drop across all events, ranked descending
- **Top Point Scorers** — by gender, handles ties, first-place finish count
- **All-Star Qualifiers** — checked against full MCSL nominating standards by age group, gender, and event (hardcoded)
- **Personal Bests** — total count of drops plus single biggest drop
- **Drop buckets** — floor-second grouping (2-second, 3-second, etc.), only drops ≥ 2 seconds
- **Export** — Copy to clipboard and Download as Markdown
- **Debug panel** — "show parsing debug info" toggle reveals event count, swimmer count, sample entries for troubleshooting
