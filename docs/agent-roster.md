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
| **GitHub** | integration | `U0B3VUN3QA1` | @github | `<@U0B3VUN3QA1>` | joined | 通知、`/github` 指令、開 issue；thread @ 可將脈絡複製至 **既有 PR** |
| **GitHub Copilot** | executor (on_demand) | （同 App `U0B3VUN3QA1`） | — | `@GitHub Copilot` | joined | Copilot **cloud agent**：讀 thread、寫 code、開 PR、建 issue 草稿（需 Copilot 訂閱） |
| **Codex** | executor | `U0B411CESCR` | @codex | `<@U0B411CESCR>` | joined | 實作、重構、可並行於 Cursor Cloud 的 coding 任務 |

> **主執行**：**Cursor**（IDE + `gh`）主控 repo 與編排。**GitHub Copilot** 在需要時由 Cursor 於 thread 明確 `@GitHub Copilot` 調用（與 `@github` 整合角色不同）。

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
4. `<@U0B3VUN3QA1>` **GitHub** — PR/Issue 建立後，於交付 thread 同步脈絡（**非** Copilot）

## 按需分派（`on_demand`）

由 Cursor 判斷後在 thread **額外** 加上（不與上表預設每次齊發）：

| 觸發 | Mention | 適用情境 |
|------|---------|----------|
| GitHub Copilot cloud agent | `@GitHub Copilot` | 希望用 **GitHub 生態** 從 Slack thread 直接寫 code / 開 PR；issue 草稿；與 `@Cursor` 擇一或分工 |
| Cursor Cloud | `@Cursor` | 已設 default repo 時，Slack 內遠端實作（見 [Cursor Slack 文件](https://cursor.com/docs/integrations/slack)） |

**GitHub Copilot 範例 prompt**（同一 thread）：

```text
@GitHub Copilot In chang180/ai-orchestrator-workflow-demo, implement the acceptance criteria in this thread. branch=feature/bootstrap-orchestrator
```

**前置**：Slack 內 GitHub App 已連結帳號、Copilot 訂閱、對 repo 有 write；於 App 設定 default repository。

## `@github` vs `@GitHub Copilot`

| | `@github` / `<@U0B3VUN3QA1>` | `@GitHub Copilot` |
|--|------------------------------|-------------------|
| 類型 | 整合、通知、輕量操作 | AI cloud agent |
| 寫 code / 開 PR | 否 | 是 |
| `/github open` 開 issue | 是 | 亦可透過對話建 issue 草稿 |
| thread → PR 脈絡 | 是（複製到既有 PR） | 是（整串 thread 當 agent 輸入） |

## 調用規則

1. **編排權與 repo 主控** 僅 Cursor IDE Agent（`gh`、branch、`docs/progress.md`）。
2. **GitHub Copilot** 為可調用的 AI executor，不取代 Cursor 編排；實作完成後 Cursor 仍負責更新 progress 與彙總。
3. 同一任務避免同時 @ `@Cursor` 與 `@GitHub Copilot` 做重複實作；擇一並在 thread 註明。
4. 執行 `notify_agents` 前以本表 `channel_status` 為準。
5. 若任務已指定單一 executor，可縮減 mention 名單。

## 更新紀錄

| 日期 | 變更 |
|------|------|
| 2026-05-15 | 初版（掃描時 bot 尚未 invite，資料不完整） |
| 2026-05-15 | 復掃：Claude、Cursor、Codex、GitHub 均已加入專案頻道 |
| 2026-05-15 | Codex 納入預設分派名單（role: executor） |
| 2026-05-15 | 拆分 GitHub（integration）與 GitHub Copilot（on_demand executor） |
