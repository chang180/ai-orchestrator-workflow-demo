# TASK T3 — GitHub Copilot / @github（v2）

**Base branch**：`test/agent-handoff-v2`  
**Repo**：`chang180/ai-orchestrator-workflow-demo`

## 必須交付

1. **PR** targeting `test/agent-handoff-v2`（Copilot 慣用交付方式）
2. **檔案**：`docs/agent-handoff/artifacts/v2/github-deliverable.md`

## 檔案內容

```markdown
---
task: T3
agent: github-copilot
status: ready_for_review
---

# GitHub Copilot deliverable (v2)

## summary
一句繁中。

## self_check
- [ ] PR base = test/agent-handoff-v2

completed_by: github-copilot at <ISO8601>
```

## 觸發句型

```text
<@U0B3VUN3QA1> In chang180/ai-orchestrator-workflow-demo, branch=test/agent-handoff-v2, read docs/agent-handoff/tasks/v2/TASK-github.md and open a PR. Do not merge.
```

## 完成時

Thread 回覆 **PR URL**。
