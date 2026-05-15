# AI Orchestrator Workflow Demo

這個 repo 示範 **Cursor 主執行** 的 AI-first 專案工作流：

- **Cursor** — 主執行 orchestrator（IDE Agent + Slack MCP + GitHub）
- **ChatGPT** — 規劃顧問（發想、PRD；不直接執行）
- **Slack** — workflow 入口、thread、通知與 audit log
- **GitHub** — repo、issue、branch、PR 與交付紀錄（`docs/progress.md`）
- **Claude / Codex / GitHub bot** — 預設分派（審閱、實作、交付連動）
- **GitHub Copilot** — 按需 `@GitHub Copilot`（Slack 內 AI 寫 code / 開 PR；需 Copilot 訂閱）

> **主控**：Cursor 編排 repo（`gh`、`docs/progress.md`）；Copilot / Codex 為可調用的 AI executor。

## Demo Goal

驗證從規劃（可經 ChatGPT）到 Slack 承接需求，再由 **Cursor** 建立 GitHub 工作項、更新進度並回報 thread 的流程。

## Important Finding

Slack Workflow Builder **不等同** GitHub Actions：repo 內 YAML 是 **contract**，不會被 Slack 自動掃描部署。

可行模式：

1. 把 workflow spec 放在 `.workflow-specs/`，版本控管。
2. 依 [slack-demo-request.md](.workflow-specs/slack-demo-request.md) **人工**在 Workflow Builder 建立入口。
3. **執行與編排由 Cursor 負責**（Slack MCP + `gh`）；`@Cursor` Cloud Agent 可從 Slack 觸發重型實作。

部分 Slack UI 功能 **無公開 API**（例如側邊欄頻道區段、列出全部已安裝 App、邀請 bot 進既有頻道）— 見 [docs/slack-capability-matrix.md](docs/slack-capability-matrix.md)。

## Slack 頻道

- `#ai-orchestrator-workflow-demo` — 專案專用（ID: `C0B40L36REE`）
- 設定：執行 `@Cursor settings` 設 default repo 為本 repo（見 runbook）

## Repository Layout

```text
.workflow-specs/
  slack-demo-request.yml      # 機器契約 v0.2
  slack-demo-request.md       # 人類 runbook

docs/
  plan.md
  architecture.md
  agent-contract.md
  agent-roster.md
  slack-capability-matrix.md
  progress.md
  workflow-builder-checklist.md

.cursor/rules/
  orchestrator.mdc
```

## 快速開始

1. 閱讀 [docs/plan.md](docs/plan.md) 與 [docs/architecture.md](docs/architecture.md)
2. 完成 [runbook Phase B](.workflow-specs/slack-demo-request.md)（`/invite` bot、`@Cursor settings`）
3. 在 Cursor 中請 agent 執行 Phase C 端到端演練

## 參考

- [Cursor Slack 整合](https://cursor.com/docs/integrations/slack)
- [Cursor Cloud Agents](https://cursor.com/docs/cloud-agent)
- [Slack Workflows](https://docs.slack.dev/workflows)
