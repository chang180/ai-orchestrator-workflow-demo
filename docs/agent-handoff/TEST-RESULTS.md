# Handoff 實測結果彙總

| 版本 | Thread | 結論 |
|------|--------|------|
| v1 | [delegation](https://devstream-core.slack.com/archives/C0B40L36REE/p1779068230291279) | 禁 PR → 無 git 交付 |
| v2 | [v2](https://devstream-core.slack.com/archives/C0B40L36REE/p1779068753832629) | Claude ✅ 審閱；Codex 部分；無標準 handoff-complete |
| T0 | [T0](https://devstream-core.slack.com/archives/C0B40L36REE/p1779069182447899) | 誤把 handoff-complete @Cursor → 日文 + PR#8（已關） |
| **v3** | [v3](https://devstream-core.slack.com/archives/C0B40L36REE/p1779069759008549) | 見下；issue [#9](https://github.com/chang180/ai-orchestrator-workflow-demo/issues/9) **已關** |
| **寫入權限** | — | PR [#10](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/10)、[#11](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/11) 已關；見下 |

## Demo 總結（2026-05-18 收尾）

| 目標 | 結果 |
|------|------|
| Cursor IDE 主控編排 | ✅ 文件、skill、E2E、人工派工 + IDE/`gh` 收尾 |
| Slack 全自動多 agent 編排 | ❌ 不在範圍；派工需人工確認（預期） |
| `handoff-complete` 協定 | ✅ v3 定稿；Claude/Codex 格式通過 |
| v2 branch 各 agent 各交 artifact PR | ❌ `artifacts/v2/` 仍空；改記錄 **fallback** |
| 建議金路徑 executor | `<@U0B3VUN3QA1>` Copilot 或 IDE 代開 PR |

**未做（可選）**：Phase D `/demo-project`、Codex 再戰 GitHub PR、@Cursor ack-only 重測。

## v3 驗證（2026-05-18）

> 教訓：**先 push PROTOCOL-v3 再發 Slack**（Test C 曾因檔案不在 main 而 blocked）。

### Test A — @Cursor ack-only

| 項目 | 預期 | 實際（~3 min） |
|------|------|----------------|
| 語言 | 繁中一句 | ❌ 無 @Cursor bot 回覆 |
| PR | 無 | ✅ 無新 PR（優於 T0） |
| 備註 | Cloud 可能排隊慢或忽略 ack-only | 編排改靠 MCP 讀 thread，不依賴 Cloud 回 ping |

### Test B — Codex execute

| 項目 | 預期 | 實際 |
|------|------|------|
| 啟動 | 開 task | ✅ |
| handoff-complete | 有、可不 @Cursor | ✅ 繁中 summary + 格式正確 |
| PR URL | 可點 | ⚠️ 環境未回傳 URL；稱已 make_pr → base `test/agent-handoff-v2` |
| GitHub 上 PR | open | ❌ 尚無（僅 Codex 環境內 commit；與 v2 相同權限問題） |

### Test C — Claude review

| 項目 | 預期 | 實際 |
|------|------|------|
| 語言 | 繁中 | ✅ |
| verdict + findings | 有 | ✅（先 blocked 檔案不在 main，後 needs_changes；合理） |
| handoff-complete 區塊 | 有 | ✅ |
| 開 PR | 無 | ✅ |

## GitHub 寫入權限（使用者自測，2026-05-18）

| PR | 目的 | 結果 |
|----|------|------|
| [#10](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/10) | Claude Code / token 寫入驗證 | ✅ 已開並關閉（測試用） |
| [#11](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/11) | MCP token PR path | ✅ 已開並關閉（測試用） |
| [#3](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/3) | Copilot README badge（早期） | 關閉 — demo 不需合併 |

main 上短暫 commit `test: mcp token write check` / cleanup 已還原，僅驗證 push 能力。

## 定稿結論（寫入 Skill）

1. **三類訊息**：execute 指派 / handoff-complete 回報 / ack-only ping（不可混用）。
2. **IDE 編排** ≠ **@Cursor Cloud**；複審循環靠 MCP + `gh`，不靠 Cloud 自動常駐。
3. **先 push 任務文件**，再 Slack handoff。
4. **一律加** `Reply in Traditional Chinese (zh-TW).`
5. **@github** 僅一個 bot；Copilot = 自然語言 + PR。
6. **Codex** 可能僅在 ChatGPT 環境 commit，PR 需權限；要 handoff-complete 即使 `blocked`；**編排 fallback**：IDE + `gh` 代開 PR。
7. **Claude** 適合 Slack 審閱 + handoff-complete 收尾；若要 push 需 fine-grained PAT（Contents/PR Read+Write）。

## 相關 issue（已關）

- [#6](https://github.com/chang180/ai-orchestrator-workflow-demo/issues/6) v1 handoff — 已棄用策略
- [#7](https://github.com/chang180/ai-orchestrator-workflow-demo/issues/7) v2 PR 交付 — artifacts 未齊，記錄 fallback
- [#9](https://github.com/chang180/ai-orchestrator-workflow-demo/issues/9) v3 協定 — 驗證完成
