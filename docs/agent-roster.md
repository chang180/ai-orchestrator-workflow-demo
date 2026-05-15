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
| **GitHub** | integration | `U0B3VUN3QA1` | @github | `<@U0B3VUN3QA1>` | joined | PR/Issue thread 同步；在 thread mention 可複製脈絡至 PR |
| **Codex** | legacy_optional | `U0B411CESCR` | @codex | `<@U0B411CESCR>` | joined | 已加入頻道；**非主 orchestrator**（預設編排不 @，除非明確指定） |

> **主執行**：僅 **Cursor**（IDE + Cloud）。Codex 留在 roster 供你手動調用，但不納入預設 workflow 分派。

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

系統訊息時間序（加入頻道）：Claude → Cursor → Codex → GitHub（約 17:45–17:46 CST）。

### 先前掃描誤差說明

初掃時頻道尚未 `/invite` bot，且若未傳 `include_bots: true`，成員列表只會顯示人類。復掃後與實際一致。

### `#proj-ai-demo`（參考）

- Claude `U0B404P284S` 亦曾於該頻道出現系統訊息

## 調用規則

1. **預設編排**：Cursor IDE → 必要時 `<@U09H5GMRSEQ>` Cloud 做重型實作。
2. **審閱**：`<@U0B404P284S>` Claude。
3. **交付通知**：`<@U0B3VUN3QA1>` GitHub（PR/Issue thread 連動）。
4. **Codex**：僅在需求明寫「請 Codex…」時 mention `<@U0B411CESCR>`；不作預設分派。
5. 執行 `notify_agents` 前以本表 `channel_status` 為準；`joined` 才可 @。

## 更新紀錄

| 日期 | 變更 |
|------|------|
| 2026-05-15 | 初版（掃描時 bot 尚未 invite，資料不完整） |
| 2026-05-15 | 復掃：Claude、Cursor、Codex、GitHub 均已加入專案頻道 |
