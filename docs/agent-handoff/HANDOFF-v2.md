# Handoff v2 — 可複審交付實驗

**編排者**：Cursor IDE Agent  
**Repo**：`chang180/ai-orchestrator-workflow-demo`  
**Branch**：`test/agent-handoff-v2`（PR **target 此 branch**，勿直接推 `main`）  
**Issue**：https://github.com/chang180/ai-orchestrator-workflow-demo/issues/7  
**Slack**：建立後寫入 `docs/progress.md`

## 與 v1 差異

| 項目 | v1 | v2 |
|------|----|----|
| Git 交付 | 勿開 PR → 多數未 push | **必須 PR** 到 `test/agent-handoff-v2` |
| Claude | 改 repo 檔案 | **僅 Slack** 結構化審閱 |
| 編排者 | 無複審手冊 | [ORCHESTRATOR-REVIEW.md](ORCHESTRATOR-REVIEW.md) |

## 子任務

| ID | Executor | 交付 | 任務文件 |
|----|----------|------|----------|
| T1 | Codex | PR + `artifacts/v2/codex-deliverable.md` | [tasks/v2/TASK-codex.md](tasks/v2/TASK-codex.md) |
| T2 | @Cursor | PR + `artifacts/v2/cursor-deliverable.md` | [tasks/v2/TASK-cursor-cloud.md](tasks/v2/TASK-cursor-cloud.md) |
| T3 | @github | PR + `artifacts/v2/github-deliverable.md` | [tasks/v2/TASK-github.md](tasks/v2/TASK-github.md) |
| T4 | Claude | **Slack thread** 審閱報告 | [tasks/v2/TASK-claude.md](tasks/v2/TASK-claude.md) |

## 共通（寫 repo 者）

1. **Base branch**：`test/agent-handoff-v2`
2. **只改** 自己的 `artifacts/v2/*-deliverable.md`（必要時可加 `.github/` 無關檔案勿動）
3. 檔案內必含 YAML front matter：

```yaml
---
task: T1|T2|T3
agent: codex|cursor-cloud|github-copilot
status: ready_for_review
---
```

4. PR 標題格式：`[handoff-v2][T1] <簡述>`
5. PR 描述須連回 Slack thread URL（編排者會貼在 handoff 訊息）

## 編排者驗收

- [ ] 最多 3 個 open PR（T1–T3）+ Claude Slack 審閱
- [ ] 執行 [ORCHESTRATOR-REVIEW.md](ORCHESTRATOR-REVIEW.md) 流程
- [ ] 在 issue #7 留言彙總
