[![Copilot cloud agent](https://github.com/chang180/ai-orchestrator-workflow-demo/actions/workflows/copilot-swe-agent/copilot/badge.svg)](https://github.com/chang180/ai-orchestrator-workflow-demo/actions/workflows/copilot-swe-agent/copilot)

# AI Orchestrator Workflow Demo

這個 repo 用來示範一個 AI-first 專案工作流：

- ChatGPT 作為 AI Orchestrator / 中控台
- Slack 作為 workflow 入口、thread、通知與 audit log
- GitHub 作為 repo、issue、branch、PR 與交付紀錄
- Cursor / Claude / Codex / Copilot 作為可被分派的 AI agent

## Demo Goal

驗證從 ChatGPT 專案發想開始，到 Slack workflow 承接需求，再到 GitHub 建立工作項目與交付檔案的流程。

## Important Finding

Slack Workflow Builder 目前不等同於 GitHub Actions：不能單純把 workflow YAML 放進 repo 後，就由 Slack 自動掃描並建立 workflow。

比較可行的模式是：

1. 把 workflow spec 放在 repo，作為可版本控管的 contract。
2. 人工或 AI agent 依照 spec 在 Slack Workflow Builder 建立流程。
3. 進階版再用 Slack App / Slack CLI / external webhook trigger 讓流程可部署、觸發或同步。

## Repository Layout

```text
.workflow-specs/
  slack-demo-request.yml
  slack-demo-request.md

docs/
  plan.md
  progress.md
  agent-contract.md
```
