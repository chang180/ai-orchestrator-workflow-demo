# 編排者複審手冊（Cursor IDE）

目標：其他 AI 依任務文件交付後，**你能 review 並接手後續**（合併、打回、開 follow-up issue）。

## 交付物類型

| Agent | 交付形式 | 你如何 review |
|-------|----------|----------------|
| Claude | **Slack thread** 結構化審閱（見 TASK-claude） | `slack_read_thread` |
| Codex | **PR** → branch `test/agent-handoff-v2` | `gh pr list` / `gh pr diff` |
| @Cursor Cloud | **PR**（同上） | 同上 |
| @github Copilot | **PR**（同上） | 同上 |

## 觸發方式（勿空等 30–60 分鐘）

Executor 完成後應在 handoff thread 發：

```text
<@U09H5GMRSEQ> handoff-complete
task: T1
status: ready_for_review
pr: https://github.com/...
```

見 [COMPLETION-REPORT.md](COMPLETION-REPORT.md)。你在 IDE 看到 Slack 通知或使用者轉述後再複審即可。

## 複審流程（v2 實驗）

```bash
# 1. 列出與實驗 branch 相關的 PR
gh pr list --repo chang180/ai-orchestrator-workflow-demo --state open

# 2. 看 branch 上檔案（即使尚無 PR）
git fetch origin test/agent-handoff-v2
git diff main...origin/test/agent-handoff-v2 -- docs/agent-handoff/artifacts/v2/

# 3. 對單一 PR
gh pr view <num> --comments
gh pr diff <num>

# 4. Slack 審閱（Claude）
# slack_read_thread channel_id=C0B40L36REE message_ts=<parent_ts>  # #11-proj-ai-orchestrator-workflow-demo
```

## 通過 / 打回標準

- **通過**：指定 `artifacts/v2/*-deliverable.md` 存在且含 `status: ready_for_review`；Claude 貼文含 `verdict: pass`。
- **打回**：PR 改錯檔、缺欄位、或 Claude `verdict: needs_changes` → 在同一 Slack thread @ 該 agent 附修正指示。
- **後續**：編排者 `gh pr merge`（若僅實驗 branch）或 cherry-pick；更新 `docs/progress.md` 與 issue。

## 禁止編排者假設

- 其他 agent 會出現在 Contributors 的「Claude / Codex」名義下（實際為你的帳號或 `copilot-swe-agent` 等 bot）。
- Slack @ 後無需複審即可 merge。
