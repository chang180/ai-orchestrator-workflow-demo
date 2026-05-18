# TASK T4 — Claude（v2，僅 Slack）

**不需** commit 進 repo。編排者要能在 Slack **review 你的輸出**。

## 必須交付

在 **handoff v2 Slack thread** 貼以下格式（繁中）：

```markdown
## Claude Review — handoff-v2

**verdict**: pass | needs_changes | blocked

**scope_reviewed**
- 已閱讀：https://github.com/chang180/ai-orchestrator-workflow-demo/blob/test/agent-handoff-v2/docs/agent-handoff/HANDOFF-v2.md

**findings**
1. …
2. …

**risks**
- …

**orchestrator_follow_up**
- 建議 Cursor 下一步：…
```

## 觸發

`<@U0B404P284S>` + 上述閱讀 URL + 產出審閱格式。**不要**開 code session、不要開 PR。

## 完成標準

`verdict` 欄位存在且 findings 至少 2 點。
