# Agent Roster

Slack workspace 內可調用的 bot／整合角色。Cursor orchestrator 執行 `slack.thread.message` 時依此表 `@mention`。

**清查方式**：`slack_list_channel_members(channel_id, include_bots=true)`（必須開啟 `include_bots`）。

**專案頻道**：`#ai-orchestrator-workflow-demo`（`C0B40L36REE`）

## Roster（專案頻道內已加入）

| 名稱 | Role | Slack user_id | Username | Mention | channel_status | 用途 |
|------|------|---------------|----------|---------|----------------|------|
| chang180 | owner | `U09HE0C7HBK` | @chang180 | `<@U09HE0C7HBK>` | joined | 決策、核准 |
| **Cursor** | orchestrator (cloud) | `U09H5GMRSEQ` | @cursor | `<@U09H5GMRSEQ>` / `@Cursor` | joined | Cloud Agent、Slack 內遠端實作 / PR |
| Cursor IDE Agent | orchestrator (local) | — | — | （Slack MCP，無 user_id） | n/a | 編排、gh、`docs/progress.md` |
| **Claude** | reviewer | `U0B404P284S` | @claude | `<@U0B404P284S>` | joined | 規格審閱、協作對話 |
| **GitHub** | integration + Copilot agent | `U0B3VUN3QA1` | @github | `<@U0B3VUN3QA1>` / `@GitHub` | joined | `/github` 通知與指令；**同一 mention** 下以自然語言任務觸發 Copilot coding agent |
| **Codex** | executor | `U0B411CESCR` | @codex | `<@U0B411CESCR>` | joined | 實作、重構 |

> **主執行**：**Cursor**（IDE + `gh`）主控 repo 與編排。GitHub Copilot **沒有**獨立 Slack bot；透過 `<@U0B3VUN3QA1>` 委派實作（見 [slack-bot-mention-tests.md](slack-bot-mention-tests.md)）。

## 頻道成員掃描紀錄

### `#ai-orchestrator-workflow-demo`（2026-05-15 復掃）

`slack_list_channel_members` + `include_bots: true` → **5 位成員**：

| # | user_id | 帳號 | 類型 |
|---|---------|------|------|
| 1 | `U09H5GMRSEQ` | @cursor | bot |
| 2 | `U09HE0C7HBK` | @chang180 | human |
| 3 | `U0B3VUN3QA1` | @github | bot |
| 4 | `U0B404P284S` | @claude | bot |
| 5 | `U0B411CESCR` | @codex | bot |

## 頻道設定前置（人工）

- Claude：頻道／workspace 內綁定本 repo
- Codex：environment 指向本 repo
- **GitHub App**：login + default repo（Copilot 與 `/github` 共用）
- 實測：[slack-bot-mention-tests.md](slack-bot-mention-tests.md)

## 預設 handoff 名單（`notify_agents`）

1. `<@U0B404P284S>` **Claude** — 審閱
2. `<@U0B411CESCR>` **Codex** — 實作

（預設**不**每次 @ GitHub；避免誤觸 Copilot 開 PR。）

## 按需分派（`on_demand`）

| 觸發 | Mention | 情境 |
|------|---------|------|
| GitHub Copilot coding agent | `<@U0B3VUN3QA1> In owner/repo, …` | Slack 內非同步寫 code / 開 PR |
| Cursor Cloud | `@Cursor` | 遠端實作（需 Cloud Agents on-demand） |
| GitHub 通知 | `/github subscribe owner/repo` | PR/Issue 推播（slash，非裸 @） |

**Copilot 範例**（已實測可開 PR）：

```text
<@U0B3VUN3QA1> In chang180/ai-orchestrator-workflow-demo, implement …. branch=feature/xxx
```

**勿用** `@GitHub Copilot` 當 Slack mention（純文字不會路由到 bot）。

## GitHub App：整合 vs Copilot

| | `/github …` | `<@U0B3VUN3QA1>` + 任務 |
|--|-------------|-------------------------|
| 訂閱 PR/Issue 通知 | 是 | 否 |
| `/github open` 開 issue | 是 | 否 |
| Copilot 非同步開 PR | 否 | 是 |
| thread → 既有 PR 脈絡 | 是 | agent 用整串 thread |

## 調用規則

1. 編排與 repo 主控僅 Cursor IDE Agent。
2. GitHub Copilot 為可調用 executor，不取代編排。
3. 同一任務勿同時 @ `@Cursor` 與 `<@U0B3VUN3QA1>` 做重複實作。
4. `notify_agents` 前查 `channel_status`。

## 更新紀錄

| 日期 | 變更 |
|------|------|
| 2026-05-15 | 初版 |
| 2026-05-18 | v2 實測 Claude/Codex |
| 2026-05-18 | 更正：Copilot 僅 `@github`／`<@U0B3VUN3QA1>`，無獨立 Copilot bot |
