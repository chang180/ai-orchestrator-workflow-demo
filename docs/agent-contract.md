# Agent 執行契約

本文件定義各角色在 workflow 中的權責與介面。主執行 orchestrator 為 **Cursor**。

## 角色定義

### ChatGPT（advisory）

- **可做**：發想、PRD、任務拆解、驗收標準、風險清單。
- **不可做**：直接寫入本 repo、直接操作 Slack/GitHub（除非人工轉貼）。
- **輸出格式**：建議以 Markdown 貼入 Slack thread，含標題、驗收條件、優先級。

### Cursor（orchestrator / executor）

- **可做**：
  - 讀寫 Slack thread（Slack MCP）
  - 建立/更新 GitHub issue、branch、PR（`gh`）
  - 更新 `docs/progress.md`
  - 依 [agent-roster.md](agent-roster.md) 發送 `@mention` 調用其他 bot
- **不可做**：
  - 假設可程式化部署 Slack Workflow Builder YAML
  - 假設可管理 Slack 側邊欄「頻道區段」
  - 未經 roster 登記即假設某 bot 已在頻道內

### Cursor Cloud（`@Cursor`）

- **觸發**：Slack `@Cursor [prompt]`，可帶 `branch=`、`autopr=`、repo 名稱。
- **可做**：遠端改 code、測試、開 PR、thread 內狀態通知。
- **與 IDE 分工**：IDE 負責編排與輕量任務；Cloud 負責重型實作（見 [architecture.md](architecture.md)）。

### Slack Workflow Builder

- **角色**：入口（shortcut、排程、表單），輸出結構化需求到頻道/thread。
- **executor**：`manual`（見 workflow spec 中 `setup_workflow_shortcut`）。

### 協作 Bot（Claude、Codex 等）

- **觸發**：Cursor 在 thread 內 `@mention`（見 roster）。
- **不可假設**：已安裝於 workspace 或已加入頻道——執行前查 roster 的 `channel_status`。

### GitHub（integration，`@github`）

- **觸發**：`<@U0B3VUN3QA1>` 或 slash `/github ...`。
- **可做**：訂閱 repo 通知、`/github open` 開 issue、關閉/重開 issue、將 **既有 PR** 的 thread 脈絡同步進 PR。
- **不可做**：取代 Cursor 編排；不等同 Copilot；不能任意建立 branch / 直接改檔（請用 Cursor + `gh`）。

### GitHub Copilot（executor on_demand，`@GitHub Copilot`）

- **觸發**：Cursor 在需要時於 thread 輸入 `@GitHub Copilot` + prompt（同一 GitHub Slack App，與 `@github` 不同 mention）。
- **可做**：Copilot cloud agent——讀取整串 thread、寫 code、開 PR、建立 issue 草稿（需 Copilot 訂閱與 repo write）。
- **分工**：Cursor 主控 repo 與進度；Copilot 負責 **委派出去的 AI 實作**；完成後由 Cursor 更新 `docs/progress.md` 並彙總。
- **參考**：[Integrating Copilot cloud agent with Slack](https://docs.github.com/copilot/how-tos/use-copilot-agents/coding-agent/integrate-coding-agent-with-slack)
- **與 @Cursor 關係**：同為 Slack 內 coding agent，同一任務擇一，避免重複開 PR。

## 步驟契約（對應 workflow spec）

| Step ID | Action | Executor | 輸入 | 輸出 |
|---------|--------|----------|------|------|
| `intake` | `slack.thread.read` | cursor | `channel_id`, `thread_ts` | 需求摘要 |
| `create_issue` | `github.issue.create` | cursor | `title`, `body` | issue URL |
| `create_branch` | `github.branch.create` | cursor | `branch_name` | branch ref |
| `notify_agents` | `slack.thread.message` | cursor | `mentions_from: agent-roster` | thread 訊息 |
| `update_progress` | `github.file.update` | cursor | `path: docs/progress.md` | commit |
| `setup_workflow_shortcut` | `slack.workflow.builder` | manual | runbook | Slack workflow ID |

## `slack.thread.message` 規則

1. 從 [agent-roster.md](agent-roster.md) 解析 `mention` 與 `role`。
2. 僅 mention `channel_status: joined` 的 bot；否則在 thread 註明需 `/invite`。
3. 預設分派：Claude、Codex、`<@U0B3VUN3QA1>`（GitHub 整合）；**GitHub Copilot** 僅在 `on_demand` 時加 `@GitHub Copilot`。
4. 訊息需含：issue 連結、branch 名、下一步、各 role 負責項；若已委派 Copilot，註明 prompt 摘要與預期 PR repo。

範例：

```markdown
**編排更新**（Cursor）
- Issue: https://github.com/chang180/ai-orchestrator-workflow-demo/issues/N
- Branch: `feature/bootstrap`
- 請 <@U0B404P284S> 審閱、<@U0B411CESCR> 實作；PR 建立後 <@U0B3VUN3QA1> 同步 thread
- （若需 GitHub 生態實作）@GitHub Copilot In chang180/ai-orchestrator-workflow-demo, ...
```

## GitHub 寫入規則

- Issue title/body 來自 Slack thread 摘要。
- `docs/progress.md` 每次狀態變更必更新（時間、issue、branch、狀態）。
- PR 描述需連回 Slack thread permalink（若有）。

## 安全與隱私

- 私有頻道搜尋使用 `slack_search_public_and_private` 前需使用者同意。
- 不在 Slack 貼 secrets、token、`.env` 內容。
