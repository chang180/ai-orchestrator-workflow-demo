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
| `@github` = Copilot | **否** → Copilot 用 `@GitHub Copilot` |

## `@github` vs `@GitHub Copilot`

| | `@github` | `@GitHub Copilot` |
|--|-----------|-------------------|
| 開 PR / 寫 code | 否 | 是 |
| `/github open` | 是 | — |
| thread 脈絡進 PR | 既有 PR | 整串 thread → agent |

## 編排衝突避免

- 主控：Cursor IDE + `gh`
- 預設 @：Claude、Codex、GitHub（整合）
- 按需 @：`@GitHub Copilot` 或 `@Cursor`（二選一）
- PR 建立後：`<@github>` 同步 thread

## 官方文件

- https://cursor.com/docs/integrations/slack
- https://cursor.com/docs/cloud-agent
- https://docs.github.com/en/integrations/how-tos/slack/use-github-in-slack
- https://docs.github.com/copilot/how-tos/use-copilot-agents/coding-agent/integrate-coding-agent-with-slack
