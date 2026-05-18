# 完成回報協定（給所有 executor）

**不要**讓編排者空等 30–60 分鐘。交付完成後 **立刻** 在 **同一 Slack handoff thread** 回報。

## 編排者是誰？

| 名稱 | Slack | 用途 |
|------|-------|------|
| **Cursor 編排** | `<@U09H5GMRSEQ>`（@Cursor） | 收到完成訊號後由人在 IDE 開 Cursor 複審，或 Cloud 代為摘要 |

> IDE 編排者不會自動常駐 Slack；**@Cursor = 喚醒通知 + 可選 Cloud 協助**，不是保證無人值守。

## 固定格式（複製貼上）

```text
<@U09H5GMRSEQ> handoff-complete

task: T1
status: ready_for_review
pr: https://github.com/chang180/ai-orchestrator-workflow-demo/pull/NNN
artifact: docs/agent-handoff/artifacts/v2/codex-deliverable.md
notes: （可選，一句繁中）
```

| 欄位 | 必填 | 說明 |
|------|------|------|
| `task` | ✅ | T1 / T2 / T3 / T4 |
| `status` | ✅ | `ready_for_review` \| `blocked` \| `failed` |
| `pr` | T1–T3 | PR URL；無 PR 填 `n/a` |
| `artifact` | T1–T3 | 交付檔路徑 |
| `notes` | 選填 | 阻礙或偏離任務說明 |

## 依角色

| Agent | 完成時 |
|-------|--------|
| Codex / @github | 開好 PR 後，**同 thread** 貼上方格式並 `@Cursor` |
| @Cursor Cloud（T2） | PR 就緒後貼 `handoff-complete`（**不要**再開新任務；僅回報） |
| Claude（T4） | 審閱貼完後加一行：`<@U09H5GMRSEQ> handoff-complete task:T4 status:ready_for_review` |

## 編排者收到後（IDE Cursor）

1. 讀 thread 的 `handoff-complete` 訊息  
2. `gh pr view` / `gh pr diff`（若有 `pr:`）  
3. 執行 [ORCHESTRATOR-REVIEW.md](ORCHESTRATOR-REVIEW.md)  
4. 回 thread 或 issue 寫複審結論  
