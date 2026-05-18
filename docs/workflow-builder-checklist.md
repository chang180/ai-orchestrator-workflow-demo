# Workflow Builder 建立清單（Phase D）

人工在 Slack 建立 `/demo-project` shortcut。完成後勾選。

## Checklist

- [ ] 開啟 Slack → **自動化** → **建立 Workflow**
- [ ] 觸發：**Shortcut**，名稱 `/demo-project`
- [ ] 限制可用頻道：`#11-proj-ai-orchestrator-workflow-demo`（建議）
- [ ] 步驟 1：收集輸入（專案名稱、需求摘要）
- [ ] 步驟 2：在 `#11-proj-ai-orchestrator-workflow-demo` 發送訊息（開 thread）
- [ ] 步驟 3（可選）：訊息含 `@Cursor in ai-orchestrator-workflow-demo, 處理此 thread`
- [ ] 發布 workflow
- [ ] 測試：執行 shortcut，確認 thread 出現且 Cursor 可讀

## 不屬於 Workflow 的範圍

- 建立 GitHub issue / branch
- 更新 `docs/progress.md`
- 多 agent 編排

詳見 [.workflow-specs/slack-demo-request.md](../.workflow-specs/slack-demo-request.md)。
