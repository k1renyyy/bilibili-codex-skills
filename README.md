# bilibili-codex-skills

Shared Codex skills for Bilibili sports operations.

This repository currently contains one skill:

```text
skills/fiba-3x3-schedule-ops
```

It standardizes recurring FIBA 3x3 schedule work, including official schedule entry, automatic station sheet creation, UTC+8 time conversion, WT/WS color formatting, team-name translation checks, and daily start/end time calculation.

## Install

Ask Codex to install the skill from this repository:

```text
请安装这个 Codex skill：
https://github.com/k1renyyy/bilibili-codex-skills/tree/main/skills/fiba-3x3-schedule-ops
```

Or install manually by copying:

```text
skills/fiba-3x3-schedule-ops
```

to:

```text
~/.codex/skills/fiba-3x3-schedule-ops
```

Restart or refresh Codex after installation.

## Usage

### Create or update a WS station

```text
使用 fiba-3x3-schedule-ops。
请根据官方链接自动在指定工作簿中创建/更新对应站次 sheet，并按 UTC+8 写入赛程。
工作簿路径：/absolute/path/to/2026 FIBA 3x3 赛程.xlsx（需从企业微信腾讯文档中导出2026 FIBA 3x3 赛程.xlsx至本地文件）
站点中文名：仙桃（根据实际站点名修改）
赛事类型：WS
WS链接：https://womensseries.fiba3x3.com/2026/xiantao/games（根据实际站点官方网址修改）
```

### Create or update a WT + WS station

```text
使用 fiba-3x3-schedule-ops。
请根据官方链接自动在指定工作簿中创建/更新对应站次 sheet，并按 UTC+8 写入赛程。
工作簿路径：/absolute/path/to/2026 FIBA 3x3 赛程.xlsx（需从企业微信腾讯文档中导出2026 FIBA 3x3 赛程.xlsx至本地文件）
站点中文名：马赛（根据实际站点名修改）
赛事类型：WT+WS
WT链接：https://worldtour.fiba3x3.com/2026/marseille/games（根据实际站点官方网址修改）
WS链接：https://womensseries.fiba3x3.com/2026/marseille/games（根据实际站点官方网址修改）
```

### 更新企业微信腾讯文档2026 FIBA 3x3 赛程文档

```text
Codex运行完毕后，将本地文档2026 FIBA 3x3 赛程.xlsx生成的新sheet粘贴进企业微信文档中。
```

### Calculate daily start/end windows

```text
使用 fiba-3x3-schedule-ops。
我现在要填入每天的开赛时间和结束时间。
开赛时间比第一场正式比赛提前 5mins，结束时间按最后一场正式比赛后 10mins。
请根据以下官方 Schedule Matrix Excel 告诉我要填入的时间，按照 UTC+8。
文件（询问负责人：Elsie）：
- /absolute/path/to/file1.xlsx
- /absolute/path/to/file2.xlsx
PS：从负责人处获得文件后可以直接粘贴进Codex对话框中，不用在手动填写文件地址
```

### 更新企业微信2026 FIBA 3x3 直播赛程 for拉流文档

```text
根据Codex对话中所得具体开赛与结束时间更新企业微信文档
```

## Operating Rules

- Always provide the real workbook path for the current user's machine.
- Use official FIBA 3x3 links or official Schedule Matrix Excel files as sources.
- Use official Excel files as the source of truth for daily start/end window calculations.
- If website and Excel sources disagree, choose the source specified by the requester or ask before editing.
- After every update, verify sheet name, game count, UTC+8 times, WT/WS color counts, date merges, and stale template content.

## Skill Details

The full workflow and department usage guide are inside:

```text
skills/fiba-3x3-schedule-ops/SKILL.md
skills/fiba-3x3-schedule-ops/references/department-usage.md
```
