# 安裝與複用

## 專案內使用（推薦給正式 AI 開發 repo）

1. 複製目錄：

   ```bash
   cp -R .cursor/skills/ai-orchestrator /path/to/your-project/.cursor/skills/
   ```

2. 可選：複製並改寫規則

   ```bash
   cp .cursor/rules/orchestrator.mdc /path/to/your-project/.cursor/rules/
   ```

3. 依 [new-project-bootstrap.md](new-project-bootstrap.md) 建立 `docs/`、`.workflow-specs/`、填 roster。

4. 在新專案對 Cursor 說：

   > 請依 ai-orchestrator skill 幫我 bootstrap 這個 repo 的 Slack + GitHub 工作流。

## 全域使用（所有專案）

```bash
cp -R .cursor/skills/ai-orchestrator ~/.cursor/skills/
```

Agent 在任一 workspace 皆可被觸發（description 關鍵字：orchestrator、Slack MCP、agent roster）。

## 與本 demo repo 的關係

| 內容 | 位置 |
|------|------|
| 可攜帶 skill（精簡流程） | `.cursor/skills/ai-orchestrator/` |
| 專案實例（真實 channel_id） | `docs/agent-roster.md` 等 |
| 自動套用的專案規則 | `.cursor/rules/orchestrator.mdc` |

正式專案：**skill 負責怎麼做；docs 負責這個專案的參數。**

Handoff 定稿請一併複製或對照：

- `handoff-protocol.md`（skill 內精簡版）
- 目標 repo 的 `docs/agent-handoff/PROTOCOL-v3.md`（可由 demo 複製後改 channel_id）

## 更新 skill

demo repo 改進 workflow 後，重新 `cp -R` 覆蓋目標專案的 skill 目錄，或 diff 合併：

- `SKILL.md`
- `handoff-protocol.md`
- `reference-slack-limits.md`
