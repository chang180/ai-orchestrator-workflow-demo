# TASK T1 — Codex（v2）

**Base branch**：`test/agent-handoff-v2`  
**Repo**：`chang180/ai-orchestrator-workflow-demo`

## 必須交付

1. **Git**：開 PR，**base = `test/agent-handoff-v2`**，head 為你的工作 branch。
2. **檔案**：`docs/agent-handoff/artifacts/v2/codex-deliverable.md`

## 檔案內容

```markdown
---
task: T1
agent: codex
status: ready_for_review
---

# Codex deliverable (v2)

## summary
一句繁中：完成了什麼。

## self_check
- [ ] 僅改本檔
- [ ] PR base 為 test/agent-handoff-v2

completed_by: codex at <ISO8601>
```

## 完成回報（必做）

PR 就緒後 **立刻** 在 handoff **同一 thread** 貼（見 [COMPLETION-REPORT.md](../../COMPLETION-REPORT.md)）：

```text
<@U09H5GMRSEQ> handoff-complete
task: T1
status: ready_for_review
pr: <PR URL>
artifact: docs/agent-handoff/artifacts/v2/codex-deliverable.md
```

## 禁止

- 不要改其他 `artifacts/v2/*`
- 不要 merge PR 自己合併
- 不要推 `main`
