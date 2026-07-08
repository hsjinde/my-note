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

## [2026-07-04] ingest | 第七批：多益文法體系（hub 導覽頁）

來源：
- [[個人學習/多益/]] 全套教材（文法 29 章 + 閱讀 6 篇 + 模擬 2 份 + 全套詳解）
- [[個人學習/多益/_specs/2026-07-03-toeic-expansion-design]]

建立概念頁 1 個：
- wiki/concepts/多益文法體系.md（章節地圖 hub，含 6 大類 29 章秒殺重點 + 全路徑 wikilink）

更新：wiki/index.md（新增「語言學習」分類）

備註：hub 型導覽頁，未逐章拆頁；如需獨立「秒殺公式速查」頁可後續再拆。

## [2026-07-05] lint | 健康檢查與修復

掃描 23 個 wiki 頁面，發現並修復：

- **異常頁**：`concepts/購物指南.md`（未登錄 index、無 ingest 紀錄、主題與 vault 無關、內部連結與來源全斷鏈）→ 移至 `.trash/`（Obsidian 回收桶，可復原）
- **孤立/欠連結**：`LLM-Wiki.md`、`知識庫架構設計.md` 提到 Karpathy 卻未連結 → 各補 `[[Andrej-Karpathy]]`（同時解除 Andrej-Karpathy 孤立狀態）；LLM-Wiki 原無任何 wiki 內部連結，新增「相關概念」導覽區塊
- **不一致**：`index.md` 中 `[[LLM-Wiki]]` 來源數 2 → 1（與頁面 frontmatter 一致）
- 已更新 LLM-Wiki、知識庫架構設計、index 的 `updated` 為 2026-07-05

未動（自然孤立，可接受）：`多益文法體系`、`KeyLogger-Server`。
待你決定：`Claude-Code.md` 模型清單可能過時（Sonnet 4.6/GPT-5.5），因屬提煉自原始筆記，未主動改。

## [2026-07-05] query+ingest | 網搜 Obsidian LLM Wiki 生態並優化筆記

來源：tvly 網路搜尋（10 個外部來源，含 MindStudio/TheToolNerd/GoPenAI/agricidaniel/GitHub second-brain）

- 新建查詢歸檔 `queries/2026-07-05-obsidian-llm-wiki.md`（補足先前空的 queries/）
- `concepts/LLM-Wiki.md`：新增「生態與實作工具」節（Karpathy 官方插件、claude-obsidian、second-brain）
- `concepts/RAG-vs-LLM-Wiki.md`：新增「為什麼 Karpathy 反對 chunked RAG」（長上下文論點）
- `index.md`：queries 區塊改為表格、登錄新歸檔頁
- `core_rules.md`：新增「⚙️ Wiki 運作紀律」（lint 節奏／query 歸檔／長上下文模型）

## [2026-07-05] query+ingest | 網搜多益秒殺文法並消化進筆記

來源：tvly 網路搜尋（13 個外部來源，含 NextSchool／PrepEdu／巨匠／TutorABC／菁英）

- `concepts/多益文法體系.md`：新增「Part 5 秒殺速查 SOP」節（四步法／位置法／字尾表／時態訊號詞／介連 8 組／兩大陷阱），並在來源補查詢歸檔連結
- 新建查詢歸檔 `queries/2026-07-05-toeic-part5-秒殺sop.md`
- `index.md`：queries 區塊新增登錄

## [2026-07-05] query+ingest | 依章節標題深挖多益進階難題（800+）

使用者回饋：先前秒殺法太初階，建議按 vault 章節標題查更難的。
來源：tvly 6 組進階網搜（15 個外部來源，含常春藤／NextSchool／PrepEdu／LTTC）

- 新建概念頁 `concepts/多益文法-進階難題.md`（假設倒裝＋混合假設、分詞構句、關代前置＋複合關代、一致/平行/比較、名詞子句/否定倒裝/意志動詞）
- 新建查詢歸檔 `queries/2026-07-05-toeic-進階文法難題.md`
- `concepts/多益文法體系.md`：教材範圍新增「進階難題」連結（雙向）
- `index.md`：concepts「語言學習」新增進階頁、queries 新增登錄

## [2026-07-09] lint | 健康檢查與修復

掃描 24 個 wiki 頁面，發現並修復：

- **斷鏈**：`concepts/NeetCode-刷題路線.md` 來源清單 `[[Best practice questions]]` 無對應檔（`個人學習/Leecode/Solution/` 下不存在）→ 移除該行
- **孤立頁**：`entities/KeyLogger-Server.md` 無任何 wiki 內部連入 → 於 `entities/Ethan.md`「相關概念」補 `[[KeyLogger-Server]]`（個人 C++ 專案），解除孤立
- 其餘 100+ 條 wikilink 全數解析正常；`mathruffian-dot…` 系列長檔名連結經核實有效（原始剪藏檔名本身即含描述文字）
