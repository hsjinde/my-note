---
title: Wiki 操作日誌
---

# Wiki 操作日誌

記錄每次對 wiki 的重要操作，格式：## [日期] 操作類型 | 說明

<!-- 範例：
## [2026-06-04] ingest | 消化 Clippings/LLM Wiki 文章
## [2026-06-04] lint | 首次健康檢查，發現 3 個孤立頁面
## [2026-06-04] query | SRE 學習路徑查詢，結果歸檔至 queries/
-->

---

## [2026-06-04] init | Wiki 知識庫初始化

- 建立 wiki/ 目錄結構：index.md、log.md、concepts/、entities/、queries/
- 建立 Clippings/Conversations/ 目錄
- 安裝 llm-wiki skill

## [2026-06-04] ingest | 第一批：obsidian相關筆記 4 篇

來源：
- [[個人學習/obsidian相關筆記/3 層架構打造個人 AI 大腦：從 Raw Data 到持久知識庫 🛠️]]
- [[個人學習/obsidian相關筆記/Karpathy 的 LLM Wiki 火了，我改造了一下，比原版好用十倍]]
- [[個人學習/obsidian相關筆記/插件安裝]]
- [[個人學習/obsidian相關筆記/目前的skills]]

建立概念頁 6 個：
- wiki/concepts/LLM-Wiki.md
- wiki/concepts/RAG-vs-LLM-Wiki.md
- wiki/concepts/知識庫架構設計.md
- wiki/concepts/Ingest-工作流.md
- wiki/concepts/Obsidian-插件.md
- wiki/concepts/Claude-Code-Skills.md

建立實體頁 4 個：
- wiki/entities/Obsidian.md
- wiki/entities/Claude-Code.md
- wiki/entities/Claudian.md
- wiki/entities/Andrej-Karpathy.md

更新：wiki/index.md

待消化：Clippings/ 4 篇、Leecode/ 1 篇、工作專案/ 1 篇、資料結構/ 5 篇 = 11 篇
## [2026-06-04] ingest | 第二批：SRE 相關 2 篇

來源：
- [[Clippings/SRE Engineer & AI Systems Developer]]
- [[個人學習/SRE 學習路徑圖.canvas]]

建立概念頁 1 個：
- wiki/concepts/SRE-學習路徑.md

建立實體頁 1 個：
- wiki/entities/Ethan.md

更新：wiki/index.md

剩餘待消化（9 篇）：Clippings/ 3 篇、Leecode/ 1 篇、工作專案/ 1 篇、資料結構/ 5 篇

## [2026-06-05] ingest | 第三批：Clippings 3 篇 + 好工具推薦

來源：
- [[Clippings/mathruffian-dotclaude-code-lazy-packs Claude Code 懶人包]]
- [[Clippings/Obsidian Skills Ai自动化笔记新方法 使用配置教程]]（內容極少，僅記錄）
- [[Clippings/Obsidian邪修用法，免费云同步，AI，手机端，还有进阶技巧]]

建立概念頁 2 個：
- wiki/concepts/Claude-Code-Lazy-Packs.md
- wiki/concepts/Obsidian-同步方案.md

建立實體頁 1 個：
- wiki/entities/mathruffian-dot.md

更新：wiki/concepts/Claude-Code-Skills.md、wiki/entities/Obsidian.md

## [2026-06-05] ingest | 第四批：資料結構鐵人挑戰 5 篇

來源：
- [[個人學習/資料結構-鐵人挑戰-35D/]] 全系列（Day1-35）

建立概念頁 4 個：
- wiki/concepts/資料結構總覽.md
- wiki/concepts/排序演算法.md
- wiki/concepts/搜尋演算法.md
- wiki/concepts/演算法策略.md

## [2026-06-05] ingest | 第五批：NeetCode 刷題路線 + 52 篇解題筆記

來源：
- [[個人學習/Leecode/NeetCode Roadmap (Full List Consolidated + Diagram)]]
- [[個人學習/Leecode/Solution/]]（52 篇解題筆記，不逐篇 ingest）
- [[個人學習/Leecode/Solution/Best practice questions]]
- [[個人學習/Leecode/Solution/50 Popular Backend Developer Interview Questions and Answers]]

建立概念頁 1 個：
- wiki/concepts/NeetCode-刷題路線.md

## [2026-06-05] ingest | 第六批：工作專案 KeyLogger

來源：
- [[工作專案/KeyLogger-Server]]

建立實體頁 1 個：
- wiki/entities/KeyLogger-Server.md

## [2026-06-05] cleanup | 清理

- 修復 log.md 重複的 SRE 批次紀錄
- 刪除 scraped_folder.html（內容為空）
- 好工具推薦/caliber-ai-org/ai-setup 內容不完整，暫不 ingest
- Clippings/Obsidian Skills 內容極少（僅 YouTube 連結），暫不 ingest
- wiki/index.md 全面更新，待消化筆記歸零

## [2026-06-07] translate | 全文翻譯 Clippings/llm-wiki.md 為繁體中文

- 將 Karpathy 原文 LLM Wiki gist 完整翻譯為繁體中文
- 覆寫 wiki/concepts/LLM-Wiki.md（原為摘要版，現改為全文翻譯版）
- 來源：[[Clippings/llm-wiki]]
