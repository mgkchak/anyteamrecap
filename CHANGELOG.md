# Changelog

All notable changes to the MCSL Meet Recap Generator.

---

## [2026-06-25] — A-Meet Team Selectors, Bug Fixes & Code Cleanup

### Added
- **A-Meet team selectors** — after pasting results, home team, away team, and "generate recap for" dropdowns appear automatically (same flow as B-meet). Works for any MCSL team — no longer defaults to Damascus.

### Fixed
- **Crash on non-DA meets** — `parseMeetPage` was missing `teams:[]` in the meet object and had a hardcoded `abbr==='DA'` check in the score parser. Both teams are now stored in `meet.teams` regardless of abbreviation, fixing a crash when neither team was Damascus.
- **Team score detection** — updated to handle both same-line (`TeamName (AB)  405.5`) and split-line (`TeamName (AB)\n405.5`) score table formats, and supports 1–3 character abbreviations.
- **B-meet PDF hint text** — updated from "Upload the HY-TEK Meet Manager PDF" to "Upload the meet recap PDF."

### Removed (dead code)
- `initTeamDropdowns` — B-meet dropdowns are now populated dynamically after upload; pre-population at page load was unused
- `fmtTime` — defined but never called
- `dz-results` / `dz-drops` drag-and-drop listeners — leftover from an earlier dual-PDF A-meet mode that no longer exists
- All non-MCSL-list hardcoded Damascus references in comments and code

---

## [2026-06-21] — B-Meet Format 2, Ligature Fixes & More

### Added
- **HY-TEK Results format support** — second B-meet PDF format (event-first, both teams listed inline) now auto-detected and parsed alongside the existing Summary format
  - Full team names (e.g. `Flower Valley Frogs`) resolved to MCSL abbreviations via `resolveTeamAbbr`
  - Handles `*N` tied places, `---` DQ rows, duplicate event numbers, page-continuation headers
  - Auto-populates home/away/recap dropdowns from teams detected in the PDF
- **"Generate recap for" dropdown on B-meet** — choose which team's recap to generate when both teams are present in the Results format PDF
- **PDF ligature normalization** — HY-TEK PDFs use Unicode ligature glyphs (`ϐ` U+03D0, `ﬁ` U+FB01, `ﬂ` U+FB02, etc.) that PDF.js splits into fragments. Fixed at three levels: item extraction, line cleanup, and stroke name matching. Fixes names like "Griffin" and events like "Butterfly" displaying as "Grif ϐ in" and "Sc meter butter ϐ ly"
- **Per-event drop tracking** — drops are now tracked individually per event (not accumulated per stroke), so two small drops in the same stroke are evaluated independently for bucket placement

### Changed
- **App title & branding** — "MCSL Meet Recap Generator"; tagline "Montgomery County Swim League · MCSL"; badge updated to 🏊
- **Tab labels** — "📋 A-Meet (Paste Results)" and "📄 B-Meet (Upload PDF)"
- **Paste instructions** — updated to exact mcsl.org navigation steps
- **Most Improved heading** — "Top 5 Most Improved (3+ Events)"
- **Biggest drop label** — "Biggest single-event drop:"
- **Meet title** — dynamically uses selected team name; no hardcoded team names
- **B-meet team dropdowns** — no pre-selection; upload PDF first, then dropdowns populate from detected teams
- **PDF extraction** — y-tolerance grouping (3px) for more accurate row detection in columnar PDFs

### Fixed
- JavaScript syntax errors from double-escaped backticks
- `flipName` duplicate last-name word artifact (e.g. `Fensterseifer Fideli, Benjamin Fideli` → `Benjamin Fensterseifer Fideli`)

### Removed
- Meet type A/B radio buttons (paste is always A-meet)
- Dead code: `parseDropsPDF`, `lookupSeed`, `dropsMap`, `radioVal`, `pdfText.results`, `pdfText.drops`
- Hardcoded "DAMASCUS" from title output

---

## [2026-06-18] — B-Meet PDF Support

### Added
- **B-Meet PDF upload mode** — HY-TEK Meet Manager Summary PDF support
  - Home and away team dropdowns with all 90+ MCSL teams
  - Parses `Last, First - Gender - Age: N` swimmer headers with `#N Event Finals SeedTime FinalTime (Place)` event lines
  - Age-to-age-group mapping derived from swimmer header
  - Handles `NT` seed times, `DQ`/`NS`/`SCR`/`EXH` finals
- **B-Meet header format** — `Week 1 B-Meet: AWAY at HOME` (no scores)
- **Top Point Scorers suppressed for B-meets**

### Fixed
- JavaScript syntax errors from backtick escaping
- `flipName` duplicate last-name word stripping

---

## [2025-06-xx] — 2025 Season

### Added
- **Most Improved filter** — requires drops in 3 or more events
- **"Top 5 Most Improved" label**
- **Week N A/B-Meet title format**

### Fixed
- `\r\n` line endings from Windows clipboard
- Points parsing `≤ 7` cap removed
- "each" wording for tied top scorers

### Initial release
- A-meet paste mode with MCSL result format parser
- Top 5 Most Improved, Top Point Scorers, All-Star Qualifiers, Personal Bests, Drop Buckets
- Copy to clipboard and Download as Markdown
- Debug panel
