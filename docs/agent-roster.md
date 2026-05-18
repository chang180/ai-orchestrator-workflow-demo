# Agent Roster

Slack workspace 內可調用的 bot／整合角色。Cursor orchestrator 執行 handoff 時依此表 `@mention`。

**Workspace**：`devstream-core`  
**清查方式**：`slack_list_channel_members(channel_id, include_bots=true)`（必須開啟 `include_bots`）。

## 頻道對照（devstream-core）

| 頻道 | Channel ID | 用途 |
|------|------------|------|
| `#00-general` | `C09HE0CEA49` | 公告、OS 導覽 |
| `#00-social` | `C09HE0CH4L9` | 社交 |
| `#10-proj-l12cv` | `C0B47UBS2HH` | 旗艦產品 l12cv |
| `#20-agent-claude` | `C0B4HTXG5PE` | Claude 審閱 |
| `#21-agent-cursor` | `C0B4E9W84LS` | Cursor Cloud |
| `#22-agent-codex` | `C0B4C7T1E94` | Codex 實作 |
| `#30-dev-github` | `C0B4ATV1WJH` | GitHub / Copilot |
| `#31-dev-release` | `C0B4C7X58VC` | 發版 |
| `#32-dev-bugs` | `C0B47UJ6V51` | 缺陷 |
| `#40-knowledge-prompts` | `C0B4EA18TC2` | Prompt 庫 |
| `#41-knowledge-architecture` | `C0B3YRZFZ47` | 架構 |
| `#42-knowledge-decisions` | `C0B3YS0GPST` | 決策存檔 |
| `#99-incubator` | `C0B4G5B0M1P` | 孵化（quota commander） |

完整分層見 [slack-channel-taxonomy.md](slack-channel-taxonomy.md)。

## Roster（bot user_id）

| 名稱 | Role | Slack user_id | Username | Mention | 專屬頻道 | channel_status |
|------|------|---------------|----------|---------|----------|----------------|
| chang180 | owner | `U09HE0C7HBK` | @chang180 | `<@U09HE0C7HBK>` | — | joined |
| **Cursor** | orchestrator (cloud) | `U09H5GMRSEQ` | @cursor | `<@U09H5GMRSEQ>` / `@Cursor` | `#21-agent-cursor` | joined |
| Cursor IDE Agent | orchestrator (local) | — | — | （Slack MCP，無 user_id） | — | n/a |
| **Claude** | reviewer | `U0B404P284S` | @claude | `<@U0B404P284S>` | `#20-agent-claude` | joined |
| **GitHub** | integration + Copilot agent | `U0B3VUN3QA1` | @github | `<@U0B3VUN3QA1>` / `@GitHub` | `#30-dev-github` | joined |
| **Codex** | executor | `U0B411CESCR` | @codex | `<@U0B411CESCR>` | `#22-agent-codex` | joined |

> **主執行**：**Cursor IDE**（Slack MCP + `gh`）主控 repo 與編排。GitHub Copilot **沒有**獨立 Slack bot；透過 `<@U0B3VUN3QA1>` 委派實作（見 [slack-bot-mention-tests.md](slack-bot-mention-tests.md)）。

## 頻道成員掃描紀錄

### Agent 專線（2026-05-18）

| 頻道 | bots 在頻道內 |
|------|----------------|
| `#20-agent-claude` | @claude |
| `#21-agent-cursor` | @cursor |
| `#22-agent-codex` | @codex, @cursor |
| `#30-dev-github` | @github |

### 遺留 `#ai-orchestrator-workflow-demo`（`C0B40L36REE`）

建議 archive；bots 已遷至上表專線。2026-05-18 復掃 → 5 位：@cursor, @chang180, @github, @claude, @codex。

## 頻道設定前置（人工）

- Claude：頻道／workspace 內綁定目標 repo（l12cv 等）
- Codex：environment 指向目標 repo
- **GitHub App**：login + default repo（Copilot 與 `/github` 共用）
- 實測：[slack-bot-mention-tests.md](slack-bot-mention-tests.md)（demo repo）

## 預設 handoff 名單（`notify_agents`）

1. `<@U0B404P284S>` **Claude** — 審閱 → `#20-agent-claude`
2. `<@U0B411CESCR>` **Codex** — 實作 → `#22-agent-codex`

（預設**不**每次 @ GitHub；避免誤觸 Copilot 開 PR。）

## 按需分派（`on_demand`）

| 觸發 | Mention | 頻道 | 情境 |
|------|---------|------|------|
| GitHub Copilot coding agent | `<@U0B3VUN3QA1> In owner/repo, …` | `#30-dev-github` | Slack 內非同步寫 code / 開 PR |
| Cursor Cloud | `@Cursor` | `#21-agent-cursor` | 遠端實作 |
| GitHub 通知 | `/github subscribe owner/repo` | `#30-dev-github` | PR/Issue 推播 |

**Copilot 範例**：

```text
<@U0B3VUN3QA1> In chang180/l12cv, implement …. branch=feature/xxx
Reply in Traditional Chinese (zh-TW).
```

## 調用規則

1. 編排與 repo 主控僅 Cursor IDE Agent。
2. GitHub Copilot 為可調用 executor，不取代編排。
3. 同一任務勿同時 @ `@Cursor` 與 `<@U0B3VUN3QA1>` 做重複實作。
4. `notify_agents` 前查目標頻道 `channel_status`（bot 是否在內）。

## 更新紀錄

| 日期 | 變更 |
|------|------|
| 2026-05-15 | 初版（demo 頻道） |
| 2026-05-18 | v2 實測 Claude/Codex |
| 2026-05-18 | 更正 Copilot mention |
| 2026-05-18 | **devstream-core** 頻道對照與 agent 專線 |
