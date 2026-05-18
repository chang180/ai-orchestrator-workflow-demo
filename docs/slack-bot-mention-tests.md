# Slack Bot @mention 實測紀錄

> **頻道**（2026-05-18）：`#11-proj-ai-orchestrator-workflow-demo`（`C0B40L36REE`）。下方 thread 連結仍有效（channel ID 不變）。

驗證「頻道／App 設定完成後」各 bot 被 `@` 時的實際行為。  
**Slack 平台**只傳遞 `app_mention` 給該 App；**是否回、怎麼回**由各產品自己決定（[官方文件](https://docs.slack.dev/reference/events/app_mention)）。

## 結論（給編排用）

| 結論 | 說明 |
|------|------|
| 帳號連結 + 頻道設定 | **必要**，否則易跑錯 repo/environment |
| `notify_agents` 實際意義 | **notify_handoff**（通知／交接），不是 sub-agent 工作佇列 |
| 主控 | 仍僅 **Cursor IDE** + `gh` |
| 各 bot 觸發 | 需**各產品格式**的明確 prompt，不能只有「請 Claude 審閱」 |
| **GitHub Copilot** | 與 **`@github` 為同一 Slack App**（`U0B3VUN3QA1`），**沒有**獨立的 `@GitHub Copilot` mention |

### GitHub App 雙模式（同一 bot）

| 模式 | 觸發方式 | 行為 |
|------|----------|------|
| **整合** | `/github subscribe`、`/github open`… | 通知、開 issue、thread→既有 PR |
| **Copilot coding agent** | `<@U0B3VUN3QA1>` 或 `@GitHub` + **自然語言任務** | 非同步實作、開 PR（[changelog](https://github.blog/changelog/2025-10-28-work-with-copilot-coding-agent-in-slack/)） |

> Slack 顯示名為 **GitHub**（username `@github`）。官方 changelog 寫 `@GitHub`；部分文件寫 `@GitHub Copilot` 是指產品名稱，**不是**第二個 Slack bot。

## 實測 v1（設定前）

**Thread**：https://devstream-core.slack.com/archives/C0B40L36REE/p1779067084858549

| Bot | 結果 |
|-----|------|
| Claude | 跳出 Select repository / Code session，未回一句話 |
| Codex | 在 **btrade** 環境開 task，非本 repo |
| `@GitHub Copilot`（純文字） | 無回覆 — **非有效 Slack mention** |
| `<@U0B3VUN3QA1>` 無任務 | 無回覆 |

## 實測 v2（頻道／App 設定就緒後）

**Thread**：https://devstream-core.slack.com/archives/C0B40L36REE/p1779067430588309

| Bot | 指令摘要 | 結果 | 備註 |
|-----|----------|------|------|
| Claude | 只回「Claude v2 OK」 | **通過** | `Working in chang180/ai-orchestrator-workflow-demo` |
| Codex | 只回「Codex v2 OK」 | **通過（附帶行為）** | 可能仍附帶開 task |
| v2-C `@GitHub Copilot` 純文字 | 無回覆 | **失敗原因**：未 @ 到 bot |
| v2-D `<@U0B3VUN3QA1>` 無任務 | 無回覆 | 預期；需自然語言任務才進 Copilot 模式 |

## 實測：Copilot 成功案例（同一 `@github`）

**Thread**：https://devstream-core.slack.com/archives/C0B40L36REE/p1779066269459759

| 指令 | 結果 |
|------|------|
| `<@U0B3VUN3QA1> Copilot In chang180/ai-orchestrator-workflow-demo, add a one-line status badge to README` | **通過** — 約 3 分鐘內開 PR #3、thread 內進度與完成通知 |

GitHub 回覆範例：「I've started a pull request…」「your pull request is ready for review」；Tip 寫明可再 mention `<@U0B3VUN3QA1|GitHub>` 迭代。

## 實測 v3（`@github` 正確 mention，僅對話）

**Thread**：https://devstream-core.slack.com/archives/C0B40L36REE/p1779067694582389

| 指令 | 結果 |
|------|------|
| `<@U0B3VUN3QA1> In chang180/ai-orchestrator-workflow-demo, 請只回覆一句繁中：「GitHub v3 OK」。不要開 PR` | **待觀察** — Copilot agent 偏任務導向；「只回一句」可能不觸發或需較久。編排驗證請用**有明確交付物**的 prompt（見下方模板）。 |

## 建議 invoke 模板

### Claude

```text
<@U0B404P284S> 請只回覆一句繁中：「…」。不要開 code session。
```

### Codex

```text
<@U0B411CESCR> 請只回覆一句繁中：「…」。使用 chang180/ai-orchestrator-workflow-demo，不要開 task。
```

### GitHub Copilot coding agent（`@github` / `@GitHub`）

```text
<@U0B3VUN3QA1> In chang180/ai-orchestrator-workflow-demo, <具體任務>. branch=feature/xxx
```

或（與 09:04 成功案例相同）：

```text
<@U0B3VUN3QA1> In chang180/ai-orchestrator-workflow-demo, implement …
```

可選參數（[官方文件](https://docs.github.com/copilot/how-tos/use-copilot-agents/coding-agent/integrate-coding-agent-with-slack)）：`repo=owner/name`、`branch=BRANCH_NAME`。

**勿使用** `@GitHub Copilot` 當 mention（Slack 不會路由到 bot）。

### GitHub 整合（非 Copilot）

```text
/github subscribe chang180/ai-orchestrator-workflow-demo
/github help
```

PR 建立後可於 thread `<@U0B3VUN3QA1>` 附上脈絡（複製至既有 PR）；訂閱用 **slash**，不要用裸 `@github subscribe`（會進 Copilot 模式並回「僅能建立 issue/PR」）。

## 前置設定清單（人工）

- [x] `/invite @github`（含 Copilot 能力，同一 App）
- [x] `@Cursor settings` → default repo
- [x] **GitHub App** DM 或 thread 內完成 login + default repo（Copilot 用）
- [x] Claude / Codex 頻道或 workspace 內綁定本 repo
- [ ] `/github subscribe chang180/ai-orchestrator-workflow-demo`（建議）

## 更新紀錄

| 日期 | 變更 |
|------|------|
| 2026-05-18 | v1 / v2 實測與文件初版 |
| 2026-05-18 | 更正：Copilot 僅能透過 `@github`／`<@U0B3VUN3QA1>`；補 09:04 成功案例與 v3 |
