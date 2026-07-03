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

## 使用方法

### 创建或更新单独 WS 站赛程（此部分可直接粘贴入Codex作为Prompt使用）

```text
使用 fiba-3x3-schedule-ops。
请根据官方链接自动在指定工作簿中创建/更新对应站次 sheet，并按 UTC+8 写入赛程。
工作簿路径：/absolute/path/to/2026 FIBA 3x3 赛程.xlsx
站点中文名：仙桃
赛事类型：WS
WS链接：https://womensseries.fiba3x3.com/2026/xiantao/games
```

说明：
- 使用前需先从企业微信腾讯文档中导出 `2026 FIBA 3x3 赛程.xlsx` 到本地。
- `工作簿路径` 替换为本地导出文件的真实路径或直接复制文档粘贴入Chat中。
- `站点中文名` 按实际站点修改，例如 `仙桃`、`马赛`、`乌兰巴托`。
- `WS链接` 替换为实际站点的官方 WS 赛程链接。

### 创建或更新 WT + WS 合并站赛程（此部分可直接粘贴入Codex作为Prompt使用）

```text
使用 fiba-3x3-schedule-ops。
请根据官方链接自动在指定工作簿中创建/更新对应站次 sheet，并按 UTC+8 写入赛程。
工作簿路径：/absolute/path/to/2026 FIBA 3x3 赛程.xlsx
站点中文名：马赛
赛事类型：WT+WS
WT链接：https://worldtour.fiba3x3.com/2026/marseille/games
WS链接：https://womensseries.fiba3x3.com/2026/marseille/games
```

说明：
- 使用前需先从企业微信腾讯文档中导出 `2026 FIBA 3x3 赛程.xlsx` 到本地。
- `工作簿路径` 替换为本地导出文件的真实路径或直接复制文档粘贴入Chat中。
- `站点中文名` 按实际站点修改。
- `WT链接` 和 `WS链接` 替换为实际站点的官方网址。

### 更新企业微信腾讯文档《2026 FIBA 3x3 赛程》（需手动进行）

```text
Codex 运行完毕后，将本地文件 2026 FIBA 3x3 赛程.xlsx 中生成的新 sheet 粘贴进企业微信腾讯文档。
```

### 计算每日开赛时间和结束时间（此部分可直接粘贴入Codex作为Prompt使用）

```text
使用 fiba-3x3-schedule-ops。
我现在要填入每天的开赛时间和结束时间。
开赛时间比第一场正式比赛提前 5mins，结束时间按最后一场正式比赛后 10mins。
请根据以下官方 Schedule Matrix Excel 告诉我要填入的时间，按照 UTC+8。
文件：
- /absolute/path/to/file1.xlsx
- /absolute/path/to/file2.xlsx
```

说明：
- 官方 Schedule Matrix Excel 文件可询问负责人 Elsie 获取。
- 从负责人处获得文件后，可以直接拖拽或粘贴进 Codex 对话框，不用手动填写文件路径。
- 如果手动填写路径，请把示例路径替换为本地真实文件路径。

### 更新企业微信腾讯文档《2026 FIBA 3x3 直播赛程 for拉流》（需手动进行）

```text
根据 Codex 对话中得到的具体开赛时间和结束时间，更新企业微信腾讯文档。
```

## Operating Rules

- Always provide the real workbook path for the current user's machine.
- Use official FIBA 3x3 links or official Schedule Matrix Excel files as sources.
- Use official Excel files as the source of truth for daily start/end window calculations.
- If website and Excel sources disagree, choose the source specified by the requester or ask before editing.
- 日常站次更新默认使用快速验证；模板变化、来源冲突或明确要求视觉检查时，再使用严格验证。
- After every update, verify sheet name, game count, UTC+8 times, WT/WS color counts, date merges, and stale template content.

## Skill Details

The full workflow and department usage guide are inside:

```text
skills/fiba-3x3-schedule-ops/SKILL.md
skills/fiba-3x3-schedule-ops/references/department-usage.md
```
