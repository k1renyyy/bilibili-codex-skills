---
name: fiba-3x3-schedule-ops
description: Use when updating FIBA 3x3 schedule workbooks from official WT, WS, World Cup, or FIBA 3x3 games pages and official Schedule Matrix Excel files. Handles automatic station sheet creation or update, UTC+8 match schedule entry, WT/WS color formatting, team-name translation checks, daily start/end time calculation, and final workbook verification.
---

# FIBA 3x3 Schedule Ops

Use this skill for recurring FIBA 3x3 schedule operations:

- Create or update station sheets in a FIBA 3x3 schedule workbook.
- Pull schedules from official FIBA 3x3 WT/WS/World Cup pages or official Schedule Matrix files.
- Convert all times to UTC+8 unless the source already states UTC+8.
- Preserve workbook style and use the established WT/WS fill colors.
- Calculate daily broadcast start/end windows.
- Verify match count, dates, times, matchups, labels, colors, and stale content before completion.

For department installation, first-time configuration, and reusable prompt examples, read `references/department-usage.md` when the user asks to distribute, install, configure, onboard teammates, or document usage.

## Operating Modes

Default to **fast mode** for routine weekly station updates:

- Use official page visible text or page state extraction, official Excel parsing, and workbook value/style checks.
- Treat UTC+8 cross-midnight schedules as normal FIBA 3x3 behavior; sort by UTC+8 datetime and rebuild date merges without escalating by default.
- Verify with workbook loadability, row count, first/last times, WT/WS D-column colors, date merges, and stale-template scan.
- Do not perform visual rendering unless the user asks, the template changed, or content/style validation indicates risk.

Use **strict mode** when any of these apply:

- New workbook template or materially changed layout.
- First time handling a new competition type.
- WT + WS combined sheet has ambiguous ordering after UTC+8 conversion, conflicting source order, or date merges cannot be determined confidently.
- Website and official Excel disagree.
- User asks for full visual/style QA.
- Fast-mode verification finds suspicious colors, merges, row counts, or stale content.

Strict mode may add visual render checks, deeper style comparison, and cross-source reconciliation. If rendering tools fail due native dependencies, report that and continue with fast-mode evidence instead of blocking routine updates.

## Source Priority

Prefer sources in this order:

1. Official Schedule Matrix Excel files supplied by the user.
2. Official FIBA 3x3 games pages supplied by the user.
3. Existing workbook sheet only as a style/template and for translation conventions.

If an official Excel file and website disagree, tell the user and ask which source to treat as authoritative unless the user already specified one.

For web pages that block direct HTTP access, use browser-rendered scraping with Playwright/Chrome. Do not rely on stale cached JSON unless the user explicitly asks for a historical snapshot.

Recommended web extraction order:

1. Inspect page state/data objects if available, such as `window.__fiba`, `window.__NUXT__`, `__NEXT_DATA__`, or similar app state.
2. If structured state is unavailable, parse the visible schedule text after the page renders.
3. If parsing is ambiguous, save a compact page text dump and compare against the visible browser page before writing.
4. If the website still cannot be parsed, ask the user for the official Schedule Matrix Excel.

## Workbook Targets and Attachments

Prefer files supplied in the current conversation before asking for manual paths. If the user attaches, drags, pastes, or mentions files in the message, use the platform-provided real file paths over placeholder text such as `/absolute/path/to/...`.

If both a placeholder path and attached files exist, attached files win. For shared department use, do not assume another user's local path.

Configuration fields to identify before editing:

- `workbook_path`: the target FIBA 3x3 workbook; may come from an attached or pasted file.
- `source_links`: one or more official WT/WS/World Cup games links.
- `source_files`: optional official Schedule Matrix Excel files; may come from attachments and do not require manual paths when available.
- `station_name_cn`: Chinese station name used for sheet naming.
- `event_types`: `WS`, `WT`, or `WT+WS`.

When multiple files are attached, classify them by filename and context:

- Main workbook: names like `2026 FIBA 3x3 赛程.xlsx`.
- WT source: names containing `WT`, `World Tour`, or `Schedule Matrix`.
- WS source: names containing `Women's Series`, `Womens Series`, or `WS`.
- Ask only when multiple plausible files remain for the same role.

Kiren's local default workbook path pattern:

- Current archived workbook: `/Users/kiren/Desktop/bilibili/项目归档/FIBA_3x3/Excel/2026 FIBA 3x3 赛程.xlsx`
- Older active workbook may appear at: `/Users/kiren/Desktop/bilibili/Excel/2026 FIBA 3x3 赛程.xlsx`

If both local paths exist, prefer the path mentioned by the user. If the user gives no path, inspect likely paths and choose the newest relevant FIBA workbook, then state which one was used. For any teammate's machine, ask for `workbook_path` if it cannot be discovered safely.

## Sheet Naming

Automatically create or update the station sheet in the specified workbook.

Naming rule:

- WS only: `站点 WS`, for example `仙桃 WS`
- WT only: `站点 WT`
- WT + WS: `站点WT WS`, for example `马赛WT WS`

If a near-duplicate typo sheet exists, such as `WT WS马塞站` when the requested sheet is `马赛WT WS`, rename or consolidate only when it is clearly the same station and the user requested the canonical name.

If `station_name_cn` is missing, infer it from the URL slug only when the mapping is obvious; otherwise ask the user for the Chinese station name before creating a sheet.

## Sheet Creation

When the target sheet does not exist:

1. Copy the header, column widths, row heights, borders, alignments, and number formats from the closest existing template:
   - Prefer a recent `WT WS...` sheet for combined WT/WS stations.
   - Prefer a recent `... WS` sheet for WS-only stations.
2. Clear old content.
3. Rebuild date-group merges in column A based on UTC+8 dates.
4. Write the schedule rows.

When the target sheet exists:

1. Preserve its table layout and styles where usable.
2. Clear stale old station rows before writing.
3. Rebuild column A date merges from scratch.

## Required Columns

Use the established workbook layout:

- A: `比赛日期（北京时间）`
- B: `开赛时间（北京时间）`
- C: `比赛性质`
- D: `比赛对阵`
- F: gender marker, usually only on the first row of each gender block

Column E is intentionally unused in the current template.

## Colors

Apply fill color to column D matchup cells:

- WT / men: `FFC7ECFF`
- WS / women: `FFFFDCC4`

Do not recolor the whole row unless the template already does. Preserve all other formatting.

## Timezone Rules

All workbook times must be UTC+8.

- If the official source says `Event (UTC+8)` or `My time (UTC+8)`, use those times directly.
- If the source provides local time and GMT:
  - France summer stops such as Marseille, Poitiers, Orleans: local UTC+2, add 6 hours.
  - Mongolia / China stops such as Ulaanbaatar, Xiantao: local UTC+8, add 0 hours.
  - Azerbaijan/Sumgait: local UTC+4, add 4 hours.
- When unsure, compare the source's GMT column to local time to infer the offset.

Store times as spreadsheet time values, not plain strings.

## Match Row Rules

Include official games only:

- Include: Qualifying Draw, Pool, Quarter-Finals/QF, Semi-Finals/SF, Final.
- Exclude: Clean Court, Session Break, Break Entertainment, Opening Ceremony, Prize Ceremony, Shoot Out, Dunk Contest, Local Amateur Games, practice, supplier/footer text.

Translate round labels:

- `Qualifying Draw A/B` -> `资格赛A组` / `资格赛B组`
- `Pool A/B/C/D` -> `小组赛A组` / `小组赛B组` / `小组赛C组` / `小组赛D组`
- `Quarter-Finals` or `QF` -> `1/4 决赛`
- `Semi-Finals` -> `半决赛`
- `Final` -> `决赛`

Translate playoff placeholders:

- `A/I` -> `A组第1`; `A/II` -> `A组第2`; same for B/C/D.
- `QD A/I` -> `资格赛A组第1`; `QD B/I` -> `资格赛B组第1`.
- `QF game 1 winner` -> `1/4决赛第1场胜者`.
- `SF game 1 winner` -> `半决赛第1场胜者`.

## Team Names

Prefer existing workbook conventions. Common translations:

- `Ub` -> `塞尔维亚Ub`
- `Neftchi SOCAR` -> `内夫特奇索卡尔`
- `Ulaanbaatar Amazons` -> `乌兰巴托亚马逊`
- `Amgalan` -> `阿木古楞`
- `Sukhbaatar` -> `苏赫巴托尔`
- `Lion City` -> `狮城`
- `Yanjing` -> `燕京`
- `Takasaki` -> `高崎`
- `Jingshi` -> `京师`
- `Rapid Bucharest` -> `布加勒斯特快速`
- `Gargzdai` / `Gargždai` -> `加尔格日代`

Keep sponsor/brand suffixes when they aid recognition, such as `RABOBANK`, `SOCAR`, `MANTINGA`, `DER STAMM`, `MMC Energy`, and `Hoptrans`.

When a city/team translation is uncertain, search for a reliable Chinese label before writing. If still uncertain, use a conservative transliteration and mention it in the final note.

## Ordering Combined WT/WS Sheets

For combined WT + WS sheets, sort all games by UTC+8 datetime across both competitions, not by source order. Cross-midnight schedules are common, especially for Europe and Americas stops, and should stay in fast mode when the converted order is clear.

Escalate to strict mode only when UTC+8 conversion leaves the order ambiguous, the official sources conflict, or date merges cannot be rebuilt confidently.

Gender marker in column F:

- Put `女子比赛` on the first row of each WS block.
- Put `男子比赛` on the first row of each WT block.

## Daily Start/End Windows

When asked to provide daily start and end times:

1. Use official Schedule Matrix Excel files if supplied.
2. Filter to official games only using the include/exclude rules above.
3. Convert to UTC+8.
4. Start time = first official game time minus 5 minutes.
5. End time = last official game time plus 10 minutes.
6. Report absolute datetimes, because many France-based schedules cross midnight in Beijing time.

Do not count Clean Court, breaks, ceremonies, Shoot Out, Dunk Contest, or Local Amateur Games as first/last games.

## Verification Before Completion

Before claiming completion, run a fresh verification. In fast mode, check:

- Target sheet exists with the expected canonical name.
- Number of written games equals official game count.
- First and last game datetime match the official source after UTC+8 conversion.
- Column D fill colors match WT/WS counts.
- Column A date merges match UTC+8 dates.
- Old station/team names from the template do not remain.
- Workbook can be loaded successfully after save.
- Workbook xlsx zip is structurally readable when possible.

In strict mode, additionally check visual render output or a deeper style snapshot when the environment supports it.

In the final response, summarize:

- Workbook path.
- Sheet name.
- Total game count and WT/WS split if relevant.
- Timezone basis.
- Any source discrepancies or uncertain translations.
