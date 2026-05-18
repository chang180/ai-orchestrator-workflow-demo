# Agent Handoff 模式（branch + 任務文件）

驗證構想：**Cursor 編排者**先建立 branch 與任務文件，再透過 Slack `@` 把**同一 branch 上的不同子任務**交給 Claude / Codex / Cursor Cloud / GitHub Copilot。

## 可行嗎？

| 做法 | 可行 | 說明 |
|------|------|------|
| Cursor 建 branch + 提交任務 `.md` 到 GitHub | ✅ | 已驗證（`gh` + git） |
| Slack 附 **branch 名 + 檔案路徑 + blob 連結** 再 @ bot | ✅ | 各 bot 讀的是 **遠端 repo**，不是本機路徑 |
| 在 Slack 寫本機 worktree 路徑（如 `.worktrees/foo`） | ❌ | Codex / Cloud / Copilot **看不到**你電腦上的目錄 |
| 本機 `git worktree` 給 **Cursor IDE** 並行編排 | ✅ | 僅編排者使用；handoff 仍用 branch 名 |
| 同一檔案同時 @ 三個 executor | ❌ | 易衝突；應**分檔**或**分階段** |
| 保證 bot 一定讀任務文件 | ⚠️ | 需明確 prompt；仍可能開 task/PR 附帶行為 |

## 正確 handoff 契約

1. **編排者（Cursor IDE）** 建立 `test/agent-handoff-delegation`（或 feature branch）。
2. 在 branch 上提交：
   - `docs/agent-handoff/HANDOFF.md`（總覽）
   - `docs/agent-handoff/tasks/TASK-*.md`（各 executor 專用）
3. `git push -u origin <branch>`。
4. 開 Slack thread，每則訊息 **只 @ 一個 bot**，含：
   - `repo=chang180/ai-orchestrator-workflow-demo`
   - `branch=test/agent-handoff-delegation`
   - 任務文件路徑與 GitHub blob URL
   - 驗收條件（改哪個檔案、不要開 PR 除非寫明）

## 本目錄

| 檔案 | 對象 |
|------|------|
| [HANDOFF.md](HANDOFF.md) | 所有人類 / agent 讀的總覽 |
| [tasks/TASK-claude.md](tasks/TASK-claude.md) | Claude |
| [tasks/TASK-codex.md](tasks/TASK-codex.md) | Codex |
| [tasks/TASK-cursor-cloud.md](tasks/TASK-cursor-cloud.md) | @Cursor Cloud |
| [tasks/TASK-github.md](tasks/TASK-github.md) | @github Copilot agent |
| `artifacts/` | 各 agent 交付物（分檔，避免衝突） |

## 實驗 branch

`test/agent-handoff-delegation`

Slack 實驗 thread：見 `docs/progress.md` 日誌（建立後更新）。
