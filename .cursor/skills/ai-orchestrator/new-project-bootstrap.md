# 新專案 Bootstrap 清單

將 AI Orchestrator 模式套用到**正式開發專案**時，依序完成。

## 1. 複製 Skill（必做）

```bash
# 專案內（與團隊共用）
cp -R /path/to/ai-orchestrator-workflow-demo/.cursor/skills/ai-orchestrator \
      /path/to/your-new-project/.cursor/skills/

# 或個人全域（所有專案可用）
cp -R /path/to/ai-orchestrator-workflow-demo/.cursor/skills/ai-orchestrator \
      ~/.cursor/skills/
```

重啟 Cursor 或重新開啟專案後，對 agent 說「使用 ai-orchestrator skill」或描述 Slack+GitHub 編排需求。

## 2. 建議 Repo 結構

```text
your-project/
├── .cursor/
│   ├── skills/ai-orchestrator/    # 從 demo 複製
│   └── rules/orchestrator.mdc     # 可從 demo 改寫 repo 名稱
├── .workflow-specs/
│   └── <project>-request.yml      # orchestrator: cursor
│   └── <project>-request.md       # runbook
├── docs/
│   ├── plan.md
│   ├── architecture.md
│   ├── agent-contract.md
│   ├── agent-roster.md            # 填入實際 channel_id、user_id
│   ├── slack-capability-matrix.md
│   └── progress.md
└── README.md
```

可從 demo repo 複製 `docs/*` 與 `.workflow-specs/*` 再改名、刪 demo 專用 ID。

## 3. Slack 設定

- [ ] 建立專案頻道（例：`#your-project-ai`）
- [ ] `/invite @Claude` `@Cursor` `@codex` `@github`（依需要）
- [ ] Slack MCP 已於 Cursor 啟用並核准
- [ ] `slack_search_channels` 取得 `channel_id`，寫入 roster 與 workflow yml
- [ ] `slack_list_channel_members` + **`include_bots: true`** 驗證 roster
- [ ] 發 Channel Charter（編排說明 + roster 連結）
- [ ] `@Cursor settings` → default repo = `owner/repo`
- [ ] `@GitHub Copilot` → login + default repo（若用 Copilot）
- [ ] Cloud Agents routing rules（可選）

## 4. GitHub 設定

- [ ] `gh auth status` 可用
- [ ] 第一個 orchestrator issue / branch 演練
- [ ] `docs/progress.md` 與 issue/PR 連結

## 5. Workflow spec 最小模板

`metadata` 必含：

```yaml
orchestrator: cursor
advisory: chatgpt
repository: owner/repo
slack_channel:
  name: your-channel
  id: Cxxxxxxxx
```

`notify_agents` 建議：

```yaml
agents: [cursor, claude, codex, github]
on_demand: [github_copilot, cursor_cloud]
```

## 6. agent-roster.md 模板列

| 名稱 | Role | Slack user_id | Mention | channel_status |
|------|------|---------------|---------|----------------|
| （owner） | owner | | | joined |
| Cursor | orchestrator (cloud) | | @Cursor | |
| Claude | reviewer | | | |
| Codex | executor | | | |
| GitHub | integration | | @github | |
| GitHub Copilot | executor (on_demand) | | @GitHub Copilot | |

**填完後用 MCP 復掃驗證，勿只靠猜測。**

## 7. 第一次 E2E 驗收

- [ ] Slack thread 貼需求
- [ ] Cursor 建 issue + branch + 更新 progress
- [ ] Slack thread 回報連結
- [ ] （可選）按需 `@GitHub Copilot` 或 `@Cursor` 實作

完成後即可在正式專案以相同模式迭代。
