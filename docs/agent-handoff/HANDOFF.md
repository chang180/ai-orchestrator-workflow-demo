# Handoff 總覽 — Agent 分工實驗

**編排者**：Cursor IDE Agent  
**Repo**：`chang180/ai-orchestrator-workflow-demo`  
**Branch**：`test/agent-handoff-delegation`  
**Issue**：https://github.com/chang180/ai-orchestrator-workflow-demo/issues/6  
**Slack**：https://devstream-core.slack.com/archives/C0B40L36REE/p1779068230291279

## 目標

驗證「先由編排者寫好任務文件 + branch，再 Slack handoff」是否比裸 @ 更可靠。

## 子任務（分檔，可並行）

| ID | Executor | 交付檔案 | 任務說明 |
|----|----------|----------|----------|
| T1 | Codex | `docs/agent-handoff/artifacts/codex-deliverable.md` | [TASK-codex.md](tasks/TASK-codex.md) |
| T2 | @Cursor Cloud | `docs/agent-handoff/artifacts/cursor-deliverable.md` | [TASK-cursor-cloud.md](tasks/TASK-cursor-cloud.md) |
| T3 | @github Copilot | `docs/agent-handoff/artifacts/github-deliverable.md` | [TASK-github.md](tasks/TASK-github.md) |

## 共通規則

1. **只改**上表指定檔案；勿改其他路徑。
2. **不要**開 PR，除非任務文件明確要求（本實驗均為 `Do not open PR`）。
3. 完成後在檔案末尾加一行：`completed_by: <agent> at <ISO8601>`。
4. 若無法讀取本 branch，回覆 Slack thread 說明阻礙。

## 編排者檢查清單

- [ ] branch 已 push
- [ ] Slack 三則 handoff 已發（各 bot 一則）
- [ ] 30–60 分鐘後檢查 `artifacts/` 與 thread 回覆
- [ ] 彙總至 `docs/progress.md`
