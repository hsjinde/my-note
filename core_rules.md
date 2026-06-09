# Core Rules

本 Vault 為 **LLM Wiki 知識管理系統**：AI 提煉原始筆記（剪藏、學習、工作） into `wiki/` 知識層，原始資料保持唯讀。

## 🛠 工具與協作
- **筆記操作優先序**：`obsidian-cli` (Skill) ➔ `mcpvault` (Obsidian MCP 備選) ➔ 檔案直接讀寫。
- **Wiki 操作**：觸發關鍵字時**優先調用** `/llm-wiki` skill（詳見 `.claude/skills/llm-wiki/SKILL.md`）。
- **Git**：禁止主動執行 commit/push/reset 等破壞性操作，除非明確要求。
- **Skills**：新增功能前先檢查 `.claude/skills/` 避免重複。

## 📁 目錄與知識架構
- `wiki/`：AI 維護的知識層（concepts/、entities/、queries/）。操作前必讀 `wiki/index.md`，操作後記錄至 `wiki/log.md`。
- **唯讀原始區**：`Clippings/`（網頁剪藏、對話紀錄）、`工作專案/`、`個人學習/`、`資料結構-鐵人挑戰-35D/` 等。AI 應提煉這些內容進 `wiki/`，而非直接大改原始檔。

## 🔑 LLM Wiki 觸發關鍵字
當使用者提及以下關鍵字時，**立即調用 `/llm-wiki` skill**：
- **Ingest / 消化 / 整理進 wiki**：將原始筆記提煉進知識庫
- **查詢 / query / 搜尋知識**：從 wiki 查詢資訊
- **健康檢查 / lint wiki**：檢查 wiki 結構問題
- **存對話 / 保存討論**：將 AI 對話存為 Clippings/Conversations/

## ✍️ 寫作與格式規範
- **極致精簡**：繁體中文回覆。短段落、精簡條列，不講廢話、不主動補充未要求的背景說明。
- **語法**：相容 Obsidian 格式。內部連結用 `[[Wikilinks]]`，外部用標準 `[Link](URL)`。
- **內容提煉**：重點在於「提煉、重組、連結知識」，而非單純美化排版或翻譯。
- **檔案操作**：優先編輯既有檔案，不建立無意義的 README 或臨時檔。

## 🛡️ 互動與安全
- **防呆確認**：需求不明確時，**先問 1 個關鍵問題**，絕不自行大量假設。
- **隱私安全**：不保存、不上傳、不擴散敏感個資與憑證。