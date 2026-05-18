# Slack Bot @mention 實測紀錄

驗證「頻道／App 設定完成後」各 bot 被 `@` 時的實際行為。  
**Slack 平台**只傳遞 `app_mention` 給該 App；**是否回、怎麼回**由各產品自己決定（[官方文件](https://docs.slack.dev/reference/events/app_mention)）。

## 結論（給編排用）

| 結論 | 說明 |
|------|------|
| 帳號連結 + 頻道設定 | **必要**，否則易跑錯 repo/environment |
| `notify_agents` 實際意義 | **notify_handoff**（通知／交接），不是 sub-agent 工作佇列 |
| 主控 | 仍僅 **Cursor IDE** + `gh` |
| 各 bot 觸發 | 需**各產品格式**的明確 prompt，不能只有「請 Claude 審閱」 |

## 實測 v1（設定前）

**Thread**：https://devstream-core.slack.com/archives/C0B40L36REE/p1779067084858549

| Bot | 結果 |
|-----|------|
| Claude | 跳出 Select repository / Code session，未回一句話 |
| Codex | 在 **btrade** 環境開 task，非本 repo |
| @github | 無回覆（預期） |
| @GitHub Copilot | 無回覆 |

## 實測 v2（頻道／App 設定就緒後）

**Thread**：https://devstream-core.slack.com/archives/C0B40L36REE/p1779067430588309

| Bot | 指令摘要 | 結果 | 備註 |
|-----|----------|------|------|
| Claude | 只回「Claude v2 OK」，不要開 session | **通過** | `Working in chang180/ai-orchestrator-workflow-demo` + `Claude v2 OK`；可能仍附 View session / Create PR 按鈕 |
| Codex | 只回「Codex v2 OK」，用本 repo | **通過（附帶行為）** | 先開 task，再回 `Codex v2 OK`；environment 已為 **chang180/ai-orchestrator-workflow-demo** |
| GitHub Copilot | `In chang180/ai-orchestrator-workflow-demo, reply…` | **本次未回覆** | 建議用完整 `In owner/repo` 句型；可再測或改 DM |
| @github | 對照 | 無回覆 | 預期；用 `/github help`、`/github subscribe` |

## 建議 invoke 模板（頻道設定完成後）

### Claude

```text
<@U0B404P284S> 請只回覆一句繁中：「…」。不要開 code session。
```

（頻道已綁 repo 時，Claude 會顯示 Working in `owner/repo`。）

### Codex

```text
<@U0B411CESCR> 請只回覆一句繁中：「…」。使用 chang180/ai-orchestrator-workflow-demo，不要開 task。
```

（Codex 仍可能自動開 task；若只要對話，需在其 App 設定限制。）

### GitHub Copilot

```text
@GitHub Copilot In chang180/ai-orchestrator-workflow-demo, <具體任務>. Do not open a PR unless asked.
```

### GitHub 整合（非 AI）

```text
/github subscribe chang180/ai-orchestrator-workflow-demo
```

PR 建立後於 thread `<@U0B3VUN3QA1>` 同步脈絡至 PR。

## 前置設定清單（人工）

- [x] `/invite` Claude、Cursor、Codex、GitHub
- [x] `@Cursor settings` → default repo
- [x] `@GitHub Copilot` login + default repo
- [x] Claude / Codex **頻道或 workspace 內綁定本 repo**（v2 實測關鍵）
- [ ] `/github subscribe`（建議，用於 PR 通知）

## 更新紀錄

| 日期 | 變更 |
|------|------|
| 2026-05-18 | v1 / v2 實測與文件初版 |
