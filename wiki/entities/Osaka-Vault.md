---
title: Osaka Vault
tags: [專案, Obsidian, MCP, LLM-Wiki, Cloudflare, R2]
updated: 2026-08-17
source_count: 1
---

# Osaka Vault — AI 驅動的旅遊知識庫

[[Ethan]] 的第二個 [[LLM-Wiki]] 實作：以 Obsidian Markdown 為底的大阪旅遊知識庫，原始剪藏層唯讀、AI 只寫 wiki 層，經 MCP 讓 Agent 讀寫，圖片走 Cloudflare R2 邊緣 CDN。規模 159+ 結構化 wiki 頁（98+ 分類餐廳、7+ 交通票券、22+ 購物商圈）。它同時是 [[Osaka-Web]] 的資料來源。

**與本 vault（my-note）的關係**：兩者共用同一套 LLM Wiki 紀律——`core_rules.md` 定規則、`wiki/log.md` 記操作、唯讀原始區、Frontmatter 契約。Osaka Vault 是把這套模式套用到旅遊領域後長出的變體。

## 技術棧

Obsidian Markdown ｜ Model Context Protocol（`@bitbonsai/mcpvault`）｜ Cloudflare R2 / Workers（`img.19980803.xyz`）｜ Defuddle Web Clipper ｜ Tavily Search/Crawl ｜ Node.js / Playwright ｜ Mermaid

## 五個工程決策

### 1. 雙層知識隔離與來源優先級

把網路抓來的「別人的食記遊記」和自己的訂單丟進同一個 context，AI 回答「我的行程是什麼」時會把別人推薦的餐廳講成你已經訂了——**這不是模型不夠好，是資料沒有分層**。作法是硬性目錄隔離：`原始資料/`（含 `別人行程/`，`source_type: reference`）與 `Osaka Trip/`（`source_type: plan`）。起草行程時必須優先載入已確認訂單（航班時刻、飯店 check-in），其餘景點只能是可替換項。

### 2. R2 資產管線與 ASCII key 正規化

剪藏圖片三條路都不好走：留本地讓 vault 體積暴增、保留外部 URL 會遇防盜鏈或失效、直接上 R2 則踩中文檔名的坑——Worker 收到請求時不對 URL 做 UTF-8 decode，`大阪城.jpg` 這種 key 一律 404。解法是自動化上傳管道強制 key 為純 ASCII 且具語意結構（`osaka/entities/osaka-castle.jpg`），綁自訂 CDN 網域，vault 本身零圖檔負擔。

### 3. MCP 原生整合

一般 AI coding assistant 對 Obsidian 筆記沒有型別意識，容易改壞 `[[Wikilinks]]` 與 YAML Frontmatter。整合 `@bitbonsai/mcpvault` 後，vault 以 [[MCP]] 暴露給 Agent，語意搜尋與 Frontmatter 驗證走型別化工具，寫入前就擋掉不符規範的欄位。

### 4. 增量消化與 7 天快取失效

每次對話重掃數百篇原始資料，token 與延遲都不能接受。改在 `wiki/log.md` 記每次消化的時間戳與檔案列表，只在三種情況重讀：`Osaka Trip/` 有變動、使用者明確要求、或距上次超過 7 天。平時直接用 `wiki/index.md` 的既有知識回答。這是 [[Ingest-工作流]] 的一種帶 TTL 的變體。

### 5. 狀態標記防幻覺

行程每個項目必須掛「已確認 ✅」或「待定 📌」。沒有確切資料時 Agent 被禁止自行填補——寧可留白標待定，也不生成看似合理的內容。解決的是「把參考資料誤植為已訂項目」這類幻覺，不是宣稱模型不會出錯。

## Wikilink 消歧義契約

同名檔（`原始資料/景點/通天閣.md` 與 `wiki/entities/景點/通天閣.md`）會讓短連結解析錯亂。規範是預設用短路徑保持可讀性，只在確有同名時改用全路徑加別名，並在表格中跳脫 `\|`。**與本 vault 歷次 lint 得到的結論相同**——見 `wiki/log.md` 2026-07-16／07-24 的撞名處理。

## 相關

- [[Ethan]] — 作者
- [[LLM-Wiki]] — 本知識庫依循的模式
- [[Osaka-Web]] — 以本知識庫為資料來源的前端儀表板
- [[MCP]] — Agent 讀寫 vault 的協定層
- [[Obsidian]]、[[Ingest-工作流]] — 平台與消化流程

## 來源

- [[工作專案/作品集/Osaka_Vault_Portfolio_Presentation|Osaka Vault 作品集簡報]]（2026-08-14，已脫敏版；出國日期、航班與住宿資訊不入 wiki）
