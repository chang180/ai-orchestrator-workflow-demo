# 執行進度

> Source of truth 與 GitHub issue/PR 同步。由 Cursor orchestrator 更新。

## 目前狀態

| 欄位 | 值 |
|------|-----|
| **Phase** | **E2E v4** — `#11-proj` 頻道 live 編排 |
| **狀態** | **completed**（T1/T2 達標；PR [#13](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/13) 為 Draft，可選合併） |
| **Branch** | `test/e2e-v4-11-proj` |
| **Issue** | [#12](https://github.com/chang180/ai-orchestrator-workflow-demo/issues/12)（可關） |
| **PR** | [#13](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/13) → base `test/e2e-v4-11-proj` |
| **Slack workspace** | `devstream-core` |
| **本 repo 頻道** | `#11-proj-ai-orchestrator-workflow-demo`（`C0B40L36REE`） |
| **最後更新** | 2026-05-18 |

## 里程碑

- [x] Phase A：文件與 spec v0.2
- [x] Phase B：bot invite、roster、@Cursor settings、Copilot 設定
- [x] Phase C：E2E 演練（issue #1 / PR #2）
- [ ] Phase D：Workflow Builder `/demo-project`（**可選**，未做）
- [x] Phase F：Agent Skill + PROTOCOL-v3 / handoff-protocol
- [x] Slack bot @mention 實測 v2 → `docs/slack-bot-mention-tests.md`
- [x] Handoff v3 協定驗證 → `docs/agent-handoff/TEST-RESULTS.md`；issues #6/#7/#9 已關
- [x] GitHub 寫入自測（PR #10、#11）；PR #3 已關（Copilot 早期實驗）
- [x] devstream-core 頻道稽核 → `docs/slack-channel-taxonomy.md`
- [x] Slack charter 訊息已發（13 頻道）；`docs/agent-roster.md` 已更新
- [x] 人工：Pin charter、Rename `#42-knowledge-decisions`、Archive `#proj-ai-demo` / `#ai-demo`（2026-05-18 復核）
- [x] 本 repo 頻道 → `#11-proj-ai-orchestrator-workflow-demo`（2026-05-18）

## Demo 結論（給下一專案）

- **主控**：Cursor IDE + `gh`；Slack 派工需**人工確認**（預期）。
- **可靠 executor**：`<@U0B3VUN3QA1>` Copilot 開 PR；Codex 常 `blocked` → IDE fallback。
- **Claude**：Slack 審閱 + `handoff-complete`；push 需 PAT。
- **勿**：把 `handoff-complete` 當任務 @Cursor（見 T0 / PR #8）。

## 日誌

### 2026-05-18（E2E v4 啟動）

- Issue [#12](https://github.com/chang180/ai-orchestrator-workflow-demo/issues/12)；branch `test/e2e-v4-11-proj` 已 push。
- Slack [E2E v4 thread](https://devstream-core.slack.com/archives/C0B40L36REE/p1779081856795149) → T1 ✅ `verdict=pass`；T2 ✅ [PR #13](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/13)（步驟化指派後 Copilot 有執行）。
- 教訓：@github 需編號步驟 + 單一檔案；`notes` 寫「請通知 Cursor IDE」利於收尾。

### 2026-05-18（收尾）

- 使用者 Slack 手動 @ 各 AI + repo 寫入測試；確認僅 IDE 可穩定主控。
- PR [#10](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/10)、[#11](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/11) 關閉（token 驗證）。
- PR [#3](https://github.com/chang180/ai-orchestrator-workflow-demo/pull/3) 關閉（Copilot badge 實驗，不合併）。
- 關閉 issues [#6](https://github.com/chang180/ai-orchestrator-workflow-demo/issues/6)、[#7](https://github.com/chang180/ai-orchestrator-workflow-demo/issues/7)、[#9](https://github.com/chang180/ai-orchestrator-workflow-demo/issues/9)。
- 更新 `TEST-RESULTS.md`、`progress.md`。

### 2026-05-18（handoff）

- **Handoff v3 協定定稿**：`PROTOCOL-v3.md`、skill `handoff-protocol.md`；[Slack v3](https://devstream-core.slack.com/archives/C0B40L36REE/p1779069759008549)。
- **Handoff v2**：branch `test/agent-handoff-v2`；artifacts 未齊。
- **Handoff v1**：branch `test/agent-handoff-delegation`；無 git 交付。
- Bot 實測 v2；PR #4、#5 已合併。

### 2026-05-15

- Bootstrap E2E、PR #2、Phase F skill；`@Cursor settings`、GitHub App login。
