# AI Orchestrator Workflow — 主計畫

## 目標

驗證 **Cursor 主執行** 的 AI-first 專案工作流：從 Slack 承接需求，經 GitHub 建立工作項與交付紀錄，並在 Slack thread 留下 audit log。

## 角色分工

| 角色 | 職責 | 執行方式 |
|------|------|----------|
| ChatGPT | 發想、PRD、驗收標準（顧問） | 人工貼入 Slack thread |
| **Cursor** | **主執行 orchestrator** | IDE Agent + Slack MCP + `gh` |
| Cursor Cloud | Slack 內 `@Cursor` 觸發的遠端分身 | Cloud Agent |
| Slack Workflow | 排程、表單、shortcut 入口 | Workflow Builder（人工建立） |
| GitHub | Issue、branch、PR、`docs/progress.md` | Source of truth |
| Claude | 審閱（reviewer） | 預設分派 |
| Codex | 實作（executor） | 預設分派 |
| GitHub bot | 交付連動（integration） | 預設分派 |
| GitHub Copilot agent | AI 實作（executor） | 按需 `<@U0B3VUN3QA1> In repo, …`（同一 `@github` App） |

## 里程碑

### Phase A — 文件與 Spec（本 repo）

- [x] `docs/*`、`.workflow-specs/slack-demo-request.md`
- [x] `slack-demo-request.yml` v0.2（`orchestrator: cursor`）
- [x] README 更新

### Phase B — Slack 與 Bot Roster

- [x] 在 `#11-proj-ai-orchestrator-workflow-demo` `/invite` 關鍵 bot（Claude、Cursor、Codex、GitHub）
- [x] `docs/agent-roster.md` 清查與維護
- [x] `@Cursor settings` 設 default repo：`chang180/ai-orchestrator-workflow-demo`
- [ ] Cloud Agents routing：`orchestrator` / `workflow` → 本 repo（**需你在 Dashboard 設定**）

### Phase C — 端到端演練

- [x] Slack thread 需求 → `gh issue` → branch → `docs/progress.md` → Slack 回報（issue #1）

### Phase D — Workflow Builder 入口

- [ ] 手動建立 `/demo-project` shortcut（見 [workflow-builder-checklist.md](workflow-builder-checklist.md)）（**需你在 Slack 執行**）

### 合併與收尾（建議）

- [x] 開 PR：`feature/bootstrap-orchestrator` → `main`（[#2](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/2)）
- [x] 合併後關閉 issue #1
- [x] Handoff v3 驗證收尾；issues #6/#7/#9 已關（見 [agent-handoff/TEST-RESULTS.md](agent-handoff/TEST-RESULTS.md)）

### Phase F — Cursor Agent Skill（可攜帶至下一專案）

- [x] `.cursor/skills/ai-orchestrator/SKILL.md` — 主 skill
- [x] `new-project-bootstrap.md` — 新專案啟用清單
- [x] `reference-slack-limits.md` — 能力邊界精簡
- [x] `INSTALL.md` — 複製至新 repo 或 `~/.cursor/skills/`

**用途**：將本 demo 的編排模式帶入下一個正式 AI 開發專案，無需重讀全部 docs。

### Phase E — 進階（後續）

- Cursor SDK 排程腳本
- 自訂 MCP（`conversations.invite`、`admin.apps.list`）

## 關鍵文件

- [architecture.md](architecture.md) — 架構與資料流
- [agent-contract.md](agent-contract.md) — 執行契約
- [slack-capability-matrix.md](slack-capability-matrix.md) — API 能力邊界
- [slack-bot-mention-tests.md](slack-bot-mention-tests.md) — @mention 實測 v1/v2
- [agent-roster.md](agent-roster.md) — Bot 與 mention 清單
- [progress.md](progress.md) — 執行進度
- [../.workflow-specs/slack-demo-request.md](../.workflow-specs/slack-demo-request.md) — 操作 runbook

## Slack 頻道

- **專案頻道**：`#11-proj-ai-orchestrator-workflow-demo`（`C0B40L36REE`）
- **參考頻道**：`#proj-ai-demo`（已有 Claude bot 紀錄）

## 成功標準

- Cursor 主執行文件齊全、E2E 演練完成、roster 與 capability matrix 可查
- **Agent Skill 可複製**：見 [.cursor/skills/ai-orchestrator/INSTALL.md](../.cursor/skills/ai-orchestrator/INSTALL.md)
