# slack-demo-request — Runbook

人類與 Cursor 依此操作；機器契約見 [slack-demo-request.yml](slack-demo-request.yml)。

## 前置條件

- Slack workspace：`devstream-core`
- 專案頻道：`#11-proj-ai-orchestrator-workflow-demo`（`C0B40L36REE`）
- Cursor：Slack MCP 已連線；GitHub `gh` 已登入 `chang180`
- Repo：`chang180/ai-orchestrator-workflow-demo`

## Phase B — 一次性設定

### 1. 邀請 Bot 進專案頻道（人工，MCP 無法代邀）

在 `#11-proj-ai-orchestrator-workflow-demo` 執行：

```text
/invite @Claude
/invite @Cursor
```

驗證：請 Cursor 執行 roster 更新，或自行確認成員列表出現 bot。

### 2. @Cursor 頻道預設 repo

在該頻道發送：

```text
@Cursor settings
```

設定 **default repository** 為：`chang180/ai-orchestrator-workflow-demo`

### 3. Cloud Agents Routing Rules

1. 開啟 [Cloud Agents Dashboard](https://www.cursor.com/dashboard/cloud-agents)
2. Routing Rules 新增：
   - `orchestrator` → `chang180/ai-orchestrator-workflow-demo`
   - `workflow` → `chang180/ai-orchestrator-workflow-demo`

### 4. 釘選 Channel Charter（可選）

由 Cursor 發送並釘選說明訊息（含本 repo、`docs/agent-roster.md` 連結）。

---

## Phase C — 端到端演練（Cursor 執行）

| 步驟 | 動作 | Executor |
|------|------|----------|
| 1 | 在專案頻道建立需求 thread（或讀既有 thread） | cursor |
| 2 | `gh issue create` | cursor |
| 3 | `git checkout -b feature/...` | cursor |
| 4 | 更新 `docs/progress.md` 並 commit | cursor |
| 5 | thread 回報 issue / branch 連結 | cursor |
| 6 | （可選）`@Cursor fix ...` 觸發 Cloud Agent | cloud |

### 需求 thread 範本

```markdown
## 需求
Bootstrap AI workflow demo — 驗證 Cursor 主控鏈路

## 驗收
- [ ] GitHub issue 已建立
- [ ] branch 已建立
- [ ] docs/progress.md 已更新
- [ ] Slack thread 已回報
```

---

## Phase D — Slack Workflow Builder（僅入口，人工）

**重要**：無法從本 repo YAML 自動部署至 Slack。

### 建立 `/demo-project` shortcut

1. Slack 左側 **自動化** → **建立** → **Workflow**
2. **觸發程序**：Shortcut → 名稱 `/demo-project`（或自訂，需與 yml `command` 一致）
3. **步驟建議**（保持簡單）：
   - 在 `#11-proj-ai-orchestrator-workflow-demo` 發送訊息（含表單欄位：專案名稱、需求摘要）
   - 可選：Reply in thread
   - 可選：訊息末尾提示 `@Cursor in ai-orchestrator-workflow-demo, 處理此 thread 需求`
4. **發布** workflow

### Workflow 不負責的事

- 建立 GitHub issue / branch（由 Cursor）
- 更新 `docs/progress.md`
- 編排多 agent（見 [agent-contract.md](../docs/agent-contract.md)）

---

## 疑難排解

| 問題 | 處理 |
|------|------|
| MCP 找不到頻道 | `slack_search_channels` 查 `ai-orchestrator` |
| Bot 無法 mention | 確認 `/invite` 與 [agent-roster.md](../docs/agent-roster.md) |
| @Cursor 無回應 | 確認 Cursor Slack app 已安裝、Cloud Agents 已啟用 |
| 想管理頻道區段 | 僅能人工；見 capability matrix `manual_only` |

## 相關文件

- [docs/plan.md](../docs/plan.md)
- [docs/agent-contract.md](../docs/agent-contract.md)
- [docs/slack-capability-matrix.md](../docs/slack-capability-matrix.md)
