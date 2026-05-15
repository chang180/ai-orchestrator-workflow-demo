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
| **Codex** | executor | `U0B411CESCR` | @codex | `<@U0B411CESCR>` | joined | 實作、重構、可並行於 Cursor Cloud 的 coding 任務 |

> **主執行**：**Cursor**（IDE + Cloud）編排。預設分派名單含 **Claude**（審閱）、**Codex**（實作）、**GitHub**（交付連動）。

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

## 預設分派名單（`notify_agents`）

依序 @mention（皆需 `channel_status: joined`）：

1. `<@U09H5GMRSEQ>` **Cursor** — 主執行／重型實作（或 IDE 已處理時略過重複 @）
2. `<@U0B404P284S>` **Claude** — 規格審閱、風險與驗收檢視
3. `<@U0B411CESCR>` **Codex** — 實作、重構、測試補強
4. `<@U0B3VUN3QA1>` **GitHub** — PR/Issue 建立後，於交付 thread 同步脈絡

## 調用規則

1. **編排權** 僅 Cursor IDE Agent；其他 bot 由 Cursor 依上表分派，不自行互搶主控。
2. 執行 `notify_agents` 前以本表 `channel_status` 為準。
3. 若任務已指定單一 executor（例如「只要 Codex」），可縮減 mention 名單並在 thread 註明。

## 更新紀錄

| 日期 | 變更 |
|------|------|
| 2026-05-15 | 初版（掃描時 bot 尚未 invite，資料不完整） |
| 2026-05-15 | 復掃：Claude、Cursor、Codex、GitHub 均已加入專案頻道 |
| 2026-05-15 | Codex 納入預設分派名單（role: executor） |
