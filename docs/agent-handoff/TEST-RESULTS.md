# Handoff 實測結果彙總

| 版本 | Thread | 結論 |
|------|--------|------|
| v1 | [delegation](https://devstream-core.slack.com/archives/C0B40L36REE/p1779068230291279) | 禁 PR → 無 git 交付 |
| v2 | [v2](https://devstream-core.slack.com/archives/C0B40L36REE/p1779068753832629) | Claude ✅ 審閱；Codex 部分；無標準 handoff-complete |
| T0 | [T0](https://devstream-core.slack.com/archives/C0B40L36REE/p1779069182447899) | 誤把 handoff-complete @Cursor → 日文 + PR#8（已關） |
| **v3** | [v3](https://devstream-core.slack.com/archives/C0B40L36REE/p1779069759008549) | 見下 |

## v3 驗證（2026-05-18，issue [#9](https://github.com/chang180/ai-orchestrator-workflow-demo/issues/9)）

> 教訓：**先 push PROTOCOL-v3 再發 Slack**（Test C 曾因檔案不在 main 而 blocked）。

### Test A — @Cursor ack-only

| 項目 | 預期 | 實際（~90s） |
|------|------|----------------|
| 語言 | 繁中一句 | _待 Cursor 回覆；90s 內無 bot 訊息_ |
| PR | 無 | 無新 PR（相對 T0 改善） |
| 備註 | `ack-only` + 勿開 PR | 已明確寫入 prompt |

### Test B — Codex execute

| 項目 | 預期 | 實際 |
|------|------|------|
| 啟動 | 開 task | ✅ On it（ChatGPT task 連結） |
| PR | base=test/agent-handoff-v2 | _進行中，待 handoff-complete_ |
| handoff-complete | 有 | _待填_ |

### Test C — Claude review

| 項目 | 預期 | 實際 |
|------|------|------|
| 語言 | 繁中 | ✅ |
| verdict + findings | 有 | ✅（blocked：PROTOCOL-v3 當時不在 main） |
| handoff-complete 區塊 | 有 | ✅ 格式正確 |
| 開 PR | 無 | ✅ |

## 定稿結論（寫入 Skill）

1. **三類訊息**：execute 指派 / handoff-complete 回報 / ack-only ping（不可混用）。
2. **IDE 編排** ≠ **@Cursor Cloud**；複審循環靠 MCP + `gh`，不靠 Cloud 自動常駐。
3. **先 push 任務文件**，再 Slack handoff。
4. **一律加** `Reply in Traditional Chinese (zh-TW).`
5. **@github** 僅一個 bot；Copilot = 自然語言 + PR。
6. **Codex** 可能僅在 ChatGPT 環境 commit，PR 需權限；要 handoff-complete 即使 `blocked`。
7. **Claude** 適合 Slack 審閱 + handoff-complete 收尾，不適合強制改 repo。
