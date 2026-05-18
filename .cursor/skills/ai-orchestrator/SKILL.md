---
name: ai-orchestrator
description: >-
  Runs Cursor-led AI orchestration with Slack (intake, threads, audit) and GitHub
  (issues, branches, progress.md) plus multi-agent handoff. Use when bootstrapping
  AI-first projects, Slack MCP + gh workflows, agent rosters, workflow specs,
  branch+task handoff to Codex/Claude/GitHub/Cursor Cloud, or applying this demo pattern.
---

# AI Orchestrator（Cursor 主控）

**Cursor IDE Agent** 為唯一主 orchestrator；ChatGPT 顧問；Slack 入口與 audit；GitHub 為 source of truth。

## 何時啟用

- Slack → Cursor → GitHub → Slack 回報
- `docs/agent-roster.md`、workflow spec、Channel Charter
- 多 agent **handoff**（branch + 任務文件 + Slack 分派）
- 關鍵字：orchestrator、handoff-complete、notify_handoff

## 核心原則

1. **Repo 主控**：issue / branch / `docs/progress.md` 僅 IDE + `gh`。
2. **`notify_agents` = notify_handoff**：不是自動 sub-agent 佇列。
3. **先 push 再 Slack**：任務 `.md` 須在 GitHub 上，否則 executor 回 `blocked`。
4. **Slack MCP**：`slack_list_channel_members` 必須 `include_bots: true`。
5. **Workflow YAML** 不會自動部署到 Slack。

## Handoff（必讀）

詳見 [handoff-protocol.md](handoff-protocol.md) 與 repo 內 `docs/agent-handoff/PROTOCOL-v3.md`。

| 要做的事 | 做法 |
|----------|------|
| 派 Codex / @github 寫 code | 指派模板 + `branch=` + PR base + **zh-TW** |
| 派 Claude 審閱 | thread 審閱格式，**勿**要求改 repo |
| 測 Slack 通路 | `@Cursor ack-only`，**勿**用 `handoff-complete` @Cursor |
| 收完工信號 | `slack_read_thread` 搜 `handoff-complete` |
| 複審 | `gh pr diff` + [ORCHESTRATOR-REVIEW](https://github.com/chang180/ai-orchestrator-workflow-demo/blob/main/docs/agent-handoff/ORCHESTRATOR-REVIEW.md) |

## 標準執行順序

```
- [ ] 1. slack_read_thread / slack_read_channel
- [ ] 2. 摘要需求與驗收
- [ ] 3. gh issue create（可選）
- [ ] 4. git checkout -b feature/...；撰寫 docs/agent-handoff/tasks/*
- [ ] 5. git push（任務文件上 GitHub）
- [ ] 6. Slack handoff（每 bot 一則，含 zh-TW）
- [ ] 7. 實作 / 或等 executor PR
- [ ] 8. 讀 handoff-complete → 複審 → progress.md → Slack/issue 回報
```

## 角色速查

| 角色 | 觸發 | 交付 |
|------|------|------|
| **Cursor IDE** | 本機 | 編排、`gh`、progress |
| **@Cursor Cloud** | Slack `@Cursor` + 任務 | PR（會幹活；`ack-only` 才 ping） |
| **@github** | `<@U0B3VUN3QA1>` + 任務 | Copilot PR（無獨立 bot 名） |
| **Codex** | `<@U0B411CESCR>` | PR 或 blocked 說明 |
| **Claude** | `<@U0B404P284S>` | Slack 審閱 + `handoff-complete` |
| **ChatGPT** | 人工貼 | 顧問，不寫 repo |

勿同時 @ `@Cursor` 與 `<@U0B3VUN3QA1>` 做同一實作。

## 文件索引（demo repo）

| 路徑 | 用途 |
|------|------|
| `docs/agent-handoff/PROTOCOL-v3.md` | Handoff 定稿協定 |
| `docs/agent-handoff/COMPLETION-REPORT.md` | 完成回報格式 |
| `docs/agent-handoff/TEST-RESULTS.md` | 實測紀錄 |
| `docs/slack-bot-mention-tests.md` | @mention 實測 |
| `docs/agent-roster.md` | user_id 與分派 |
| `docs/agent-contract.md` | 步驟契約 |

## 複製到新專案

見 [INSTALL.md](INSTALL.md)、[new-project-bootstrap.md](new-project-bootstrap.md)。

## 陷阱

見 [reference-slack-limits.md](reference-slack-limits.md)。
