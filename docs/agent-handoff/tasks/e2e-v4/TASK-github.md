# TASK T2 — GitHub Copilot（E2E v4）

**你是 coding agent（@github）**，不是一般聊天。請**只做下面一件事**，完成後在 Slack thread 回報。

**Repo**：`chang180/ai-orchestrator-workflow-demo`  
**工作分支**：`test/e2e-v4-11-proj`（在此分支上 commit）  
**PR 目標分支（base）**：`test/e2e-v4-11-proj`（**不是** `main`）

---

## 你要做的事（僅此一項）

**新增一個檔案**：`docs/agent-handoff/E2E-V4-STATUS.md`

內容必須包含：

```markdown
# E2E v4 狀態

| 步驟 | 狀態 | 備註 |
|------|------|------|
| feature 發起 | done | Issue #12 |
| T1 Claude 審閱 | done | verdict=pass |
| T2 本 PR | done | （填你的 PR 編號或 URL） |

Channel: #11-proj-ai-orchestrator-workflow-demo (C0B40L36REE)
```

然後 **開 Pull Request**：

- PR **into**（base）=`test/e2e-v4-11-proj`
- PR **from** 你的工作分支（含上述新檔案的 commit）
- **不要** merge 到 `main`

---

## 不要做

- 不要改 README、不要改其他無關檔案
- 不要 merge PR
- 不要在 Slack 只回「好的」而不開 PR

---

## Slack 完成回報（貼在同一 thread，繁中）

```text
handoff-complete
task: T2
status: ready_for_review
pr: https://github.com/chang180/ai-orchestrator-workflow-demo/pull/數字
artifact: docs/agent-handoff/E2E-V4-STATUS.md
notes: 已完成。請通知 Cursor IDE 編排者複審此 PR。
```

**最後一行請原文包含**：`已完成，請通知 Cursor IDE 編排者複審。`

---

## Slack 觸發（編排者用）

```text
<@U0B3VUN3QA1> Reply in Traditional Chinese (zh-TW).

You are the GitHub Copilot coding agent. Do exactly TASK T2:

1. In repo chang180/ai-orchestrator-workflow-demo, checkout branch test/e2e-v4-11-proj.
2. Create ONLY the file docs/agent-handoff/E2E-V4-STATUS.md (content见 TASK-github.md on that branch).
3. Commit and push.
4. Open a PR with BASE branch test/e2e-v4-11-proj (NOT main).
5. Reply in this Slack thread with handoff-complete (see TASK-github.md) and say: 已完成，請通知 Cursor IDE 編排者複審。

Task file: https://github.com/chang180/ai-orchestrator-workflow-demo/blob/test/e2e-v4-11-proj/docs/agent-handoff/tasks/e2e-v4/TASK-github.md
```
