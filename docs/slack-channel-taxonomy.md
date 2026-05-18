# Slack 頻道分類（devstream-core）

**Workspace**：`devstream-core`  
**目的**：將 Slack 作為 AI Engineering OS 的 command center、audit log、handoff bus、decision archive。

## 分層模型

| Layer | 前綴 | 角色 |
|-------|------|------|
| 0 | `00-` | 全 workspace 通用 |
| 1 | `10-proj-` | 產品線（一產品一頻道） |
| 2 | `20-agent-` | Agent 專線（handoff bus） |
| 3 | `30-dev-` / `31-` / `32-` | 工程、發版、缺陷 |
| 4 | `40-knowledge-` / `41-` / `42-` | 知識與決策存檔 |
| 9 | `99-` | 孵化／實驗 |

## 標準頻道登錄（2026-05-18 稽核）

| 頻道 | Channel ID | 成員數 | 建議 |
|------|------------|--------|------|
| `#00-general` | `C09HE0CEA49` | — | **keep** — charter 已發（2026-05-18） |
| `#00-social` | `C09HE0CH4L9` | 1 | **keep** |
| `#10-proj-l12cv` | `C0B47UBS2HH` | 1 | **keep** — 旗艦產品 l12cv |
| `#20-agent-claude` | `C0B4HTXG5PE` | 2 | **keep** — @claude 已加入 |
| `#21-agent-cursor` | `C0B4E9W84LS` | 2 | **keep** — @cursor 已加入 |
| `#22-agent-codex` | `C0B4C7T1E94` | 3 | **keep** — @codex + @cursor |
| `#30-dev-github` | `C0B4ATV1WJH` | 2 | **keep** — @github 已加入 |
| `#31-dev-release` | `C0B4C7X58VC` | 2 | **keep** — 含 @asana |
| `#32-dev-bugs` | `C0B47UJ6V51` | 1 | **keep** |
| `#40-knowledge-prompts` | `C0B4EA18TC2` | 1 | **keep** |
| `#41-knowledge-architecture` | `C0B3YRZFZ47` | 1 | **keep** |
| `#42-knowledge-decisions` | `C0B3YS0GPST` | 1 | **keep** — 已 rename（2026-05-18） |
| `#99-incubator` | `C0B4G5B0M1P` | 1 | **keep** — quota commander 等待孵化 |

## 非標準／遺留頻道

| 頻道 | Channel ID | 成員數 | 建議 |
|------|------------|--------|------|
| `#proj-ai-orchestrator-workflow-demo` | `C0B40L36REE` | — | **可選 archive** — 已 rename（原 `#ai-orchestrator-workflow-demo`），仍為 active；bots 已遷 Layer 2/3 |
| `#proj-ai-demo` | `C0B3UAM8199` | — | **已歸檔**（MCP 搜尋不可見，2026-05-18 復核） |
| `#ai-demo` | `C0B4UV4RJ3A` | — | **已歸檔**（MCP 搜尋不可見，2026-05-18 復核） |

## 訊息標籤（Channel Charter）

### Layer 1 — `#10-proj-l12cv`

| 標籤 | 用途 |
|------|------|
| `[feature]` | 功能需求、規格討論 |
| `[bug]` | 缺陷回報 |
| `[release]` | 版本、上線 |
| `[ux]` | 體驗、設計 |
| `[infra]` | 基礎建設、部署 |

### Layer 2 — `#20-agent-*`

| 標籤 | 用途 |
|------|------|
| `[ask]` | 單次問答、不需開 PR |
| `[handoff]` | 編排指派（含 repo / branch / 驗收） |
| `[handoff-complete]` | Executor 完成回報（見 PROTOCOL-v3） |

### Layer 4 — `#42-knowledge-decisions`

```text
Decision: （一句決策）
Why: （背景）
Tradeoff: （取捨）
```

## Bot 與頻道對應（建議最小集）

| 頻道 | 應在頻道內的 bot |
|------|------------------|
| `#20-agent-claude` | @claude |
| `#21-agent-cursor` | @cursor |
| `#22-agent-codex` | @codex（@cursor 可選，利於 Cloud 協作） |
| `#30-dev-github` | @github |

完整 user_id 見 [agent-roster.md](agent-roster.md)。

## 產品對照

| 產品 | 頻道 | 狀態 |
|------|------|------|
| **l12cv** | `#10-proj-l12cv` | 旗艦 |
| **quota commander** | `#99-incubator`（建議） | 孵化中，尚無專屬頻道 |

## 更新紀錄

| 日期 | 變更 |
|------|------|
| 2026-05-18 | 初版：devstream-core 全頻道稽核 |
| 2026-05-18 | 修正 `#00-general` ID；Slack charter 訊息已發 |
| 2026-05-18 | 復核：rename / Pin / 遺留歸檔完成；demo 頻道改為 `proj-ai-orchestrator-workflow-demo` |
