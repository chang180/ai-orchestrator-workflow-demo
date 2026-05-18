---
name: ai-orchestrator
description: >-
  Runs Cursor-led AI orchestration with Slack (intake, threads, audit) and GitHub
  (issues, branches, progress.md) plus multi-agent dispatch. Use when bootstrapping
  an AI-first project, setting up Slack MCP + gh workflows, agent rosters, workflow
  specs, or applying the ai-orchestrator-workflow-demo pattern to a new repository.
---

# AI Orchestrator（Cursor 主控）

本 skill 來自 [ai-orchestrator-workflow-demo](https://github.com/chang180/ai-orchestrator-workflow-demo)。**Cursor IDE Agent 為唯一主 orchestrator**；ChatGPT 僅顧問；Slack 為入口與 audit；GitHub 為 source of truth。

## 何時啟用

- 新專案要套用「Slack → Cursor → GitHub → Slack 回報」
- 要建立或更新 `docs/agent-roster.md`、workflow spec、Channel Charter
- 要清查 Slack bot、分派 Claude/Codex/Copilot
- 使用者提到：AI workflow、orchestrator、Slack MCP、多 agent 編排

## 核心原則

1. **Repo 主控**：issue / branch / `docs/progress.md` 由 Cursor + `gh` 完成，不假設其他 bot 能編排。
2. **Slack MCP 掃 bot**：`slack_list_channel_members` 必須 `include_bots: true`。
3. **Workflow YAML ≠ Slack 部署**：`.workflow-specs/*.yml` 是 contract；Workflow Builder 需人工或 Deno SDK。
4. **禁止假設**：MCP 可 invite bot、管理頻道區段、列出 workspace 全部 App。

## 標準執行順序

```
- [ ] 1. 讀 Slack thread / 頻道（slack_read_thread / slack_read_channel）
- [ ] 2. 摘要需求與驗收條件
- [ ] 3. gh issue create（若需要）
- [ ] 4. git checkout -b feature/...
- [ ] 5. 實作 + 更新 docs/progress.md
- [ ] 6. commit / push；必要時 gh pr create
- [ ] 7. Slack thread 回報（issue、branch、PR、下一步）
- [ ] 8. 依 roster 分派 bot（預設 vs on_demand）
```

## 角色速查

| 角色 | 觸發 | 用途 |
|------|------|------|
| Cursor IDE | 本機對話 | 編排、`gh`、progress |
| `@Cursor` | Slack | Cloud Agent 實作 / PR |
| `@github` | `<@USER_ID>` / slash | 通知、`/github open`、thread→既有 PR |
| `@GitHub Copilot` | Slack 文字 | Copilot cloud agent（按需） |
| Claude / Codex | `<@USER_ID>` | 審閱、實作（預設分派） |
| ChatGPT | 人工貼上 | 顧問，不寫入 spec orchestrator |

**同一任務勿同時 @ `@Cursor` 與 `@GitHub Copilot` 做重複實作。**

## 新專案啟用

完整檔案樹、roster 模板、一次性設定清單見 [new-project-bootstrap.md](new-project-bootstrap.md)。

## 能力邊界與陷阱

Slack / GitHub / Cursor 分工表見 [reference-slack-limits.md](reference-slack-limits.md)。  
@mention 實測與 invoke 模板：專案內見 `docs/slack-bot-mention-tests.md`。

## 本 repo 對照文件

若當前 workspace 即 demo repo，優先讀：

- `docs/agent-roster.md` — 實際 user_id 與分派名單
- `docs/agent-contract.md` — 步驟契約
- `.workflow-specs/slack-demo-request.yml` — 機器契約
- `.cursor/rules/orchestrator.mdc` — 專案規則

## 複製到下一個專案

見 [INSTALL.md](INSTALL.md)：複製整個 `.cursor/skills/ai-orchestrator/` 至新 repo 或 `~/.cursor/skills/`。
