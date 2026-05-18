---
name: slack-orchestrator
description: >-
  Audits and governs devstream-core Slack as an AI Engineering OS: channel taxonomy,
  bot roster, message conventions, handoff templates. Use when onboarding devstream-core,
  auditing Slack channels, inviting bots, or posting [handoff] / [feature] tagged messages.
---

# Slack Orchestrator（devstream-core）

**Workspace**：`devstream-core`（`devstream-core.slack.com`）  
**主 orchestrator**：Cursor IDE Agent（Slack MCP 以 `@chang180` 身分）  
**旗艦產品**：l12cv · **孵化中**：quota commander（僅 `#99-incubator`）

## 何時啟用

- 新頻道 / 重新命名 / 歸檔決策
- Bot 是否在 agent 頻道內（`include_bots: true`）
- 訊息是否符合 channel charter（`[feature]`、`[handoff]`、`Decision:` 等）
- 撰寫或複製 handoff thread 模板

## 執行順序（稽核）

```
1. slack_search_channels（多組 query：00-、10-proj、20-agent…）
2. slack_list_channel_members（include_bots: true）
3. slack_read_channel（各 charter 頻道最近 30 則）
4. 更新 docs/slack-*.md 與本 skill
```

## 頻道分層（摘要）

| Layer | 頻道 | 用途 |
|-------|------|------|
| 0 | `00-general`, `00-social` | 公告、社交 |
| 1 | `10-proj-*` / `11-proj-*` | 產品討論（如 `10-proj-l12cv`、`11-proj-ai-orchestrator-workflow-demo`） |
| 2 | `20-agent-claude`, `21-agent-cursor`, `22-agent-codex` | Agent handoff bus |
| 3 | `30-dev-github`, `31-dev-release`, `32-dev-bugs` | 工程與發版 |
| 4 | `40-knowledge-prompts`, `41-knowledge-architecture`, `42-knowledge-decisions` | 知識庫 |
| 9 | `99-incubator` | 孵化（quota commander 等） |

完整對照與 channel ID：[docs/slack-channel-taxonomy.md](../../../docs/slack-channel-taxonomy.md)

## 訊息契約

### `#10-proj-l12cv`

開頭標籤：`[feature]` `[bug]` `[release]` `[ux]` `[infra]`

### `#20-agent-*`

`[ask]` `[handoff]` `[handoff-complete]`（完成回報結構見 PROTOCOL-v3）

### `#42-knowledge-decisions`

```text
Decision:
Why:
Tradeoff:
```

## Handoff 模板

見 [docs/slack-handoff-template.md](../../../docs/slack-handoff-template.md) 與 `docs/agent-handoff/PROTOCOL-v3.md`。

**必寫**：`Reply in Traditional Chinese (zh-TW).`  
**禁止**：把 `handoff-complete` 當成 @Cursor 的 execute 工作單。

## Bot roster（workspace 級）

| Bot | user_id | 專屬頻道 |
|-----|---------|----------|
| @cursor | `U09H5GMRSEQ` | `#21-agent-cursor` |
| @claude | `U0B404P284S` | `#20-agent-claude` |
| @codex | `U0B411CESCR` | `#22-agent-codex` |
| @github | `U0B3VUN3QA1` | `#30-dev-github` |

邀請 bot：**Slack MCP 無法** `/invite` → 頻道內人工執行。

## MCP 限制

見 [docs/slack-capability-matrix.md](../../../docs/slack-capability-matrix.md)。  
`slack_list_channel_members` 一律 `include_bots: true`。

## 與 ai-orchestrator skill 的關係

- **ai-orchestrator**：跨 repo 編排、gh、PROTOCOL-v3、demo 頻道實測
- **slack-orchestrator**：devstream-core **工作區結構**與 charter 執行

## 稽核紀錄

| 日期 | 項目 | 狀態 |
|------|------|------|
| 2026-05-18 | 初版治理稽核、charter 發送 | done |
| 2026-05-18 | Pin / rename `#42-knowledge-decisions` / 遺留歸檔 | done（復核 score **88**） |
| 2026-05-18 | 本 repo → `#11-proj-ai-orchestrator-workflow-demo` | done |

**主頻道**：`#00-general` = `C09HE0CEA49`
