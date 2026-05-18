# Agent Handoff（branch + 任務文件）

## 版本

| 版本 | Branch | 說明 |
|------|--------|------|
| v1 | `test/agent-handoff-delegation` | 初版；禁止 PR → 幾乎無 git 交付 |
| **v2** | `test/agent-handoff-v2` | **必須 PR** + Claude Slack 審閱 |
| **v3** | （協定） | [PROTOCOL-v3.md](PROTOCOL-v3.md) · [TEST-RESULTS.md](TEST-RESULTS.md) |

**目前請用 v3 協定**：[PROTOCOL-v3.md](PROTOCOL-v3.md) · 實作 branch 仍可用 v2 的 `test/agent-handoff-v2`

## 核心原則

1. 編排者 push **branch + TASK.md** 到 GitHub。
2. **寫 code 的 agent**（Codex / Cursor / @github）→ **開 PR** 到實驗 branch。
3. **Claude** → **Slack 結構化審閱**（編排者用 thread review）。
4. 編排者依 [ORCHESTRATOR-REVIEW.md](ORCHESTRATOR-REVIEW.md) 做 `gh pr diff`、merge 或打回。

Contributors 不會出現「Claude/Codex」帳號；會是 `chang180`、`copilot-swe-agent`、`cursoragent` 等。
