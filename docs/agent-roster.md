# Agent Roster

Slack workspace 內可調用的 bot／整合角色。Cursor orchestrator 執行 `slack.thread.message` 時依此表 `@mention`。

**清查方式**：MCP 頻道成員掃描 + 公開頻道 bot 系統訊息反查 + 管理後台手動補登（見 [slack-capability-matrix.md](slack-capability-matrix.md)）。

**專案頻道**：`#ai-orchestrator-workflow-demo`（`C0B40L36REE`）

## Roster

| 名稱 | Role | Slack user_id | Mention | channel_status | 用途 |
|------|------|---------------|---------|----------------|------|
| Cursor IDE Agent | orchestrator | — | （本機 MCP，無 mention） | n/a | 編排、gh、progress |
| Cursor Cloud | executor | （App mention `@Cursor`） | `@Cursor` | **需 invite** | Slack 觸發遠端 coding / PR |
| Claude | reviewer | `U0B404P284S` | `<@U0B404P284S>` | **not_in_project_channel**（在 `#proj-ai-demo`） | 規格審閱、協作 |
| chang180 | owner | `U09HE0C7HBK` | `<@U09HE0C7HBK>` | joined | 決策、核准 |

## 頻道成員掃描紀錄

### `#ai-orchestrator-workflow-demo`（2026-05-15）

- `U09HE0C7HBK` @chang180（人類）
- 無 bot 成員（`include_bots: true` 仍僅 1 人）→ **請執行 `/invite @Claude`、`/invite @Cursor`**

### `#proj-ai-demo`（參考）

- Claude 系統訊息：`Claude is now available in this channel. Tag <@U0B404P284S|Claude>`

## 待手動補登（Workspace 管理應用程式）

在 Slack **設定 → 管理應用程式** 查看後填入：

| 名稱 | Role | user_id | channel_status | 備註 |
|------|------|---------|----------------|------|
| GitHub | integration | | | |
| （其他已安裝 app） | | | | |

## 調用規則

1. 執行 `notify_agents` 前檢查 `channel_status`。
2. 若為 `not_in_project_channel`，thread 內提示：`/invite @AppName`。
3. 勿 mention 已出局角色（如 Codex）。

## 更新紀錄

| 日期 | 變更 |
|------|------|
| 2026-05-15 | 初版；Claude 自 #proj-ai-demo 反查；專案頻道尚無 bot |
