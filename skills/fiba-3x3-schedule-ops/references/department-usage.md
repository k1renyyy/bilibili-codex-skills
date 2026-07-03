# Department Usage

Use this reference when packaging, sharing, installing, or configuring `fiba-3x3-schedule-ops` for teammates.

## What To Share

Share the whole folder:

```text
skills/
└── fiba-3x3-schedule-ops/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    └── references/
        └── department-usage.md
```

Zip the `skills/fiba-3x3-schedule-ops` folder or place it in a shared internal repository. The receiving user should keep the folder path unchanged.

## Installation

Each user places the folder here:

```text
~/.codex/skills/fiba-3x3-schedule-ops
```

If the distributed zip extracts to `skills/fiba-3x3-schedule-ops`, copy or move only `fiba-3x3-schedule-ops` into `~/.codex/skills/`.

Then restart or refresh Codex so the skill list is reloaded.

## First-Time Configuration

Ask each user to identify their own workbook path. Do not reuse another user's absolute path.

Minimum configuration needed in prompts:

```text
工作簿路径：/absolute/path/to/2026 FIBA 3x3 赛程.xlsx
站点中文名：仙桃
赛事类型：WS
官方链接：https://womensseries.fiba3x3.com/2026/xiantao/games
```

For WT + WS:

```text
工作簿路径：/absolute/path/to/2026 FIBA 3x3 赛程.xlsx
站点中文名：马赛
赛事类型：WT+WS
WT链接：https://worldtour.fiba3x3.com/2026/marseille/games
WS链接：https://womensseries.fiba3x3.com/2026/marseille/games
```

For daily start/end windows:

```text
根据这些官方 Schedule Matrix Excel，告诉我每天的开赛时间和结束时间，按 UTC+8。
开赛时间比第一场正式比赛提前 5 分钟，结束时间按最后一场正式比赛后 10 分钟。
文件：
- /absolute/path/to/WT Marseille 2026 - Schedule Matrix.xlsx
- /absolute/path/to/FIBA 3x3 Women's Series Marseille Stop 2026 - Schedule Matrix.xlsx
```

## Standard User Prompts

Create or update a WS station:

```text
使用 fiba-3x3-schedule-ops。
请根据官方链接自动在指定工作簿中创建/更新对应站次 sheet，并按 UTC+8 写入赛程。
工作簿路径：/absolute/path/to/2026 FIBA 3x3 赛程.xlsx
站点中文名：仙桃
赛事类型：WS
WS链接：https://womensseries.fiba3x3.com/2026/xiantao/games
```

Create or update a WT + WS station:

```text
使用 fiba-3x3-schedule-ops。
请根据官方链接自动在指定工作簿中创建/更新对应站次 sheet，并按 UTC+8 写入赛程。
工作簿路径：/absolute/path/to/2026 FIBA 3x3 赛程.xlsx
站点中文名：马赛
赛事类型：WT+WS
WT链接：https://worldtour.fiba3x3.com/2026/marseille/games
WS链接：https://womensseries.fiba3x3.com/2026/marseille/games
```

Calculate daily windows:

```text
使用 fiba-3x3-schedule-ops。
我现在要填入每天的开赛时间和结束时间。开赛时间比第一场正式比赛提前 5mins，结束时间按最后一场正式比赛后 10mins。
请根据以下官方 Schedule Matrix Excel 告诉我要填入的时间，按照 UTC+8。
文件：
- /absolute/path/to/file1.xlsx
- /absolute/path/to/file2.xlsx
```

## Department Operating Rules

- Always provide the workbook path unless the workbook is already attached in the conversation.
- Always provide official links or official Schedule Matrix files.
- Use official Excel files as source of truth when the task is daily start/end windows.
- If the website and official Excel disagree, choose the source the requester specified; otherwise ask before editing.
- Keep one workbook as the shared source of truth. Avoid multiple copies unless a copy is explicitly for backup.
- After every update, require a final summary with sheet name, game count, WT/WS count, timezone basis, and any uncertain translations.

## Troubleshooting

- If the FIBA web page returns 403, use browser-rendered scraping through Playwright/Chrome.
- If a sheet is blank, copy layout from the closest previous WS or WT+WS sheet.
- If a sheet exists with a typo name, consolidate only when it clearly refers to the same station.
- If a date crosses midnight after UTC+8 conversion, split the column A date merge by the Beijing date.
- If a team translation is uncertain, prefer existing workbook conventions, then search for a reliable Chinese label.
