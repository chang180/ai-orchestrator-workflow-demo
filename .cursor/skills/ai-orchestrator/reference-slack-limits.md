# Slack / 整合能力邊界（精簡）

## Cursor 三條路徑

| 路徑 | 觸發 | 擅長 |
|------|------|------|
| IDE + Slack MCP | Cursor 內 | 編排、讀寫 thread、`gh` |
| `@Cursor` | Slack | Cloud Agent、PR |
| `@cursor/sdk` | cron/CI | 進階自動化 |

## Slack MCP 常見誤判

| 以為可以 | 實際 |
|----------|------|
| 邀請 bot 進頻道 | **不行** → `/invite` 人工 |
| 列出全部已安裝 App | **不行** → 成員掃描 + 手動補登 |
| 管理側邊欄頻道區段 | **不行** → manual_only |
| repo YAML 部署 Workflow | **不行** → Workflow Builder 人工 |
| 有獨立的 `@GitHub Copilot` bot | **否** → 與 `@github` 同一 App（`U0B3VUN3QA1`） |

## GitHub App 雙模式（同一 `@github`）

| 模式 | 觸發 | 行為 |
|------|------|------|
| 整合 | `/github subscribe`、`/github open`… | 通知、issue、thread→既有 PR |
| Copilot agent | `<@U0B3VUN3QA1>` + 自然語言任務 | 非同步寫 code、開 PR |

## 編排衝突避免

- 主控：Cursor IDE + `gh`
- 預設 @：Claude、Codex
- 按需實作：`<@U0B3VUN3QA1> In repo, …` 或 `@Cursor`（二選一）
- 訂閱通知：`/github subscribe`（slash）

## 官方文件

- https://cursor.com/docs/integrations/slack
- https://cursor.com/docs/cloud-agent
- https://docs.github.com/en/integrations/how-tos/slack/use-github-in-slack
- https://docs.github.com/copilot/how-tos/use-copilot-agents/coding-agent/integrate-coding-agent-with-slack
- https://github.blog/changelog/2025-10-28-work-with-copilot-coding-agent-in-slack/
