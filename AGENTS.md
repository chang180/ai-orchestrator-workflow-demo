# AGENTS.md

## Cursor Cloud specific instructions

本 repo 是**純文件 + Cursor skills/rules 專案**（AI orchestrator 可攜 bootstrap kit），
**沒有應用程式原始碼、套件管理檔、建置系統或自動化測試框架**。因此不需要 `npm install`、
`pip install` 之類的依賴安裝；VM 內建的 `git`、`python3`（含 PyYAML）、`node` 即足以開發。

### 專案本質
- 「產品」= 可攜的 v4 local-first 編排 kit：把 `.cursor/skills/` 複製到新 repo，再用
  `.cursor/skills/ai-orchestrator/bootstrap.md` 依 `templates/manifest.json` 產生 `docs/` 與
  `.cursor/rules/orchestrator.mdc`（模板含 `{{...}}` placeholder，需全部替換）。
- 交付 SoT = `docs/progress.md` + `docs/tasks/*.md`，以 `gh` / git push 更新（見
  `docs/WORKFLOW-v4.md` 與 `.cursor/rules/orchestrator.mdc`）。

### Lint / Test（非標準；repo 無 CI/lint 設定）
- 沒有既有測試指令。等效的 lint/test = 驗證結構化檔案可解析且 manifest 引用完整：
  所有 `*.json` 用 `json`、`*.yml` 用 PyYAML 解析，並確認 `templates/manifest.json` 的
  `required`/`optional` 每個 `template` 檔都存在。可用一次性 Python 腳本完成（勿假設 repo 內有測試框架）。

### Build / Run（核心功能）
- 「build/run」= 執行 bootstrap 渲染：讀 `templates/manifest.json`，替換 placeholder，
  把輸出寫到**目標新 repo**。驗證時請渲染到暫存目錄（例如 `/tmp/target-repo`），
  **切勿**渲染覆蓋本 demo repo 自己的 `docs/`（bootstrap.md 明確要求：新專案勿複製 demo `docs/`）。
- 沒有需要長駐的服務（無 dev server / DB / port）。

### 陷阱
- 用 Python `glob("**/*")` 掃描時**不會**進入以點開頭的隱藏目錄（`.cursor`、`.workflow-specs`），
  會漏掉 `manifest.json` 與 `.tpl`／`.yml`；請改用 `os.walk`（並排除 `.git`）。
