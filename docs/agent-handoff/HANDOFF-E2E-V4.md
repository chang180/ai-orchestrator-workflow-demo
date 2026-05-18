# Handoff E2E v4 — `#11-proj-ai-orchestrator-workflow-demo`

**Repo**：`chang180/ai-orchestrator-workflow-demo`  
**Branch（PR base）**：`test/e2e-v4-11-proj`  
**Slack 頻道**：`#11-proj-ai-orchestrator-workflow-demo`（`C0B40L36REE`）

## 目標

驗證 devstream-core 新 Layer 1 專案頻道上的完整編排鏈路：

`[feature]` → IDE 建 issue/branch/task → push → `[handoff]` → executor → `handoff-complete` → 複審

## 任務

| ID | 執行者 | 交付 |
|----|--------|------|
| T1 | Claude | Slack thread 審閱本文件與任務範圍（review-only） |
| T2 | @github Copilot | 新增 `docs/agent-handoff/E2E-V4-STATUS.md` 並開 PR |

## 驗收（整體）

- [ ] T1：`verdict: pass` 且 findings ≥ 2
- [ ] T2：PR 指向 `test/e2e-v4-11-proj`，含 `E2E-V4-STATUS.md`
- [ ] 兩者皆在 Slack 貼 `handoff-complete`

## 任務文件

- [tasks/e2e-v4/TASK-claude.md](tasks/e2e-v4/TASK-claude.md)
- [tasks/e2e-v4/TASK-github.md](tasks/e2e-v4/TASK-github.md)
