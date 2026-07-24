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

## [2026-07-24] ingest | 新增 PA440 防火牆情資自動化配置工具技術分享

來源：[[工作專案/PA440-FW-data-configurator.md]] (`D:\PA440-FW-data-configurator`)
新增專案筆記：[[工作專案/PA440-FW-data-configurator.md]]
新增實體頁：[[wiki/entities/PA440-FW-data-configurator]]
更新索引：[[wiki/index.md]]

## [2026-07-24] ingest | 新增 Matt Pocock Skills for Real Engineers

來源：使用者提問與檢索資料提煉
新增概念頁：[[Matt-Pocock-Skills-for-Real-Engineers]]
新增實體頁：[[Matt-Pocock]]
更新索引：[[wiki/index.md]]

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
- [[Clippings/職涯/SRE Engineer & AI Systems Developer]]
- [[個人學習/SRE 學習路徑圖.canvas]]

建立概念頁 1 個：
- wiki/concepts/SRE-學習路徑.md

建立實體頁 1 個：
- wiki/entities/Ethan.md

更新：wiki/index.md

剩餘待消化（9 篇）：Clippings/ 3 篇、Leecode/ 1 篇、工作專案/ 1 篇、資料結構/ 5 篇

## [2026-06-05] ingest | 第三批：Clippings 3 篇 + 好工具推薦

來源：
- [[Clippings/AI 開發工具/mathruffian-dotclaude-code-lazy-packs Claude Code 懶人包：每支教學影片附一個 MD 檔，丟給 Claude Code 就能自動完成。]]
- [[Clippings/Obsidian/Obsidian Skills Ai自动化笔记新方法 使用配置教程]]（內容極少，僅記錄）
- [[Clippings/Obsidian/Obsidian邪修用法，免费云同步，AI，手机端，还有进阶技巧]]

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
- 來源：[[Clippings/AI 開發工具/llm-wiki]]

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

## [2026-07-15] lint | 健康檢查與修復（本 session 於 vault 安裝 note-maintain skill 後首跑）

掃描 26 個 wiki 內容頁（16 concepts＋7 entities＋3 queries）。孤立頁 0；`.canvas` 連結（SRE 學習路徑圖、NeetCode Roadmap）經查證存在、非斷鏈。

**自動修（格式）**：
- `entities/Ethan.md`：IEEE 論文連結誤用 `[[網址|別名]]` wikilink 語法 → 改外部連結 `[IEEE Xplore](https://ieeexplore.ieee.org/document/10230082)`
- `concepts/Ingest-工作流.md`：ingest 範本區塊圍欄為單反引號（未成程式碼區塊），佔位符 `[[現有wiki頁面]]` 外洩成連結 → 改三反引號程式碼區塊

**規則檔同步**：`CLAUDE.md`、`AGENTS.md` 均仍為指向 `core_rules.md` 的薄指標檔，無重複規則、無矛盾；本次 `core_rules.md` 未改動。

**待決定（未動）**：
- `entities/Andrej-Karpathy.md`：`[[范凱說AI]]`、`[[HC AI說人話]]` 斷鏈（無對應頁）→ 建實體頁 or 去連結化
- `entities/Claude-Code.md`：模型清單過時（Sonnet 4.6／GPT-5.5）→ 更新為現行 or 保留
- 未消化 Clippings 2 篇：`How I build with AI as a 1-person product team`、`Codex 逆袭开始！…OpenAI Codex…` → 是否 ingest（若 ingest 再更新 index）

## [2026-07-16] lint | 健康檢查與修復

掃描 28 個 wiki 頁面（26 內容頁）＋全 vault 原始區盤點。

**已修（自動）**

- **格式**：`concepts/Ingest-工作流.md` 步驟 2 的方案範本誤用單反引號當程式碼圍欄（應為三反引號），使範本內佔位符 `[[現有wiki頁面]]` 成為活的斷鏈 → 改為 ` ```text ` 圍欄；範本正常呈現，斷鏈解除
- **索引失準**：`index.md`「待消化筆記」原記「全部消化完畢」，實際仍有 13 篇未提煉 → 補上完整清單（Clippings 2、個人學習 2、工作專案 7、好工具推薦 2）
- **索引補漏**：`index.md` 原始資料位置表補上 `個人學習/多益/`、`個人學習/LLM與AI/`、`日常/`、`靈感/`；`好工具推薦/` 說明更新（舊述之 caliber-ai-org 路徑已不存在，現為 2 篇工具設定指南）
- `index.md` updated → 2026-07-16

**檢查結果**

- 孤立頁：**0**（26 個內容頁全數有 wiki 內入鏈）
- 斷鏈：全量複驗後實際僅餘 4 條（2 條待決、2 條屬歷史紀錄刻意不改）

**待決（未動，等使用者裁示）**

- `entities/Andrej-Karpathy.md`：`[[范凱說AI]]`、`[[HC AI說人話]]` 無對應頁（外部自媒體帳號）
- `entities/Claude-Code.md`：模型清單過時 — 「Sonnet 4.6（預設）」「GPT-5.5」既非實際模型，亦未見於來源筆記（`目前的skills`／`插件安裝` 只提到 MiniMax 2.7、GLM、千問、DeepSeek、OpenRouter）。自 2026-07-05 lint 起懸置
- `CLAUDE.md`：「Skills 檢查」「Git 紀律」兩條與 `core_rules.md` 重複（內容一致但違反單一真相來源）
- `log.md` 內 2 條 `Best practice questions` 斷鏈：屬 2026-06-05／2026-07-09 歷史紀錄，**刻意保留**（日誌為 append-only 事實紀錄，不回頭改寫）

**工具備註（下次 lint 必看）**

Windows PowerShell 5.1 的 `Get-Content` 預設以 ANSI 讀 UTF-8，中文全部變亂碼 → 首版腳本誤報 87 條斷鏈、14 個孤立頁。改用 `[System.IO.File]::ReadAllText($p, UTF8Encoding)` 讀取，並額外處理表格內跳脫管線 `\|`（`[[路徑\|別名]]`）後，實際只有 5 條。日後寫 lint 腳本務必沿用此兩點。

## [2026-07-16] lint | 矛盾與衝突處理（第二輪）

首輪漏做 lint 的「矛盾與過時」項，補做後另發現兩類問題。

**內容矛盾（已調和）**

- `concepts/RAG-vs-LLM-Wiki.md` **同頁自相矛盾**：「為什麼 Karpathy 反對 chunked RAG」說不要 RAG 切塊，下一節「混合使用」卻說可疊加 RAG 向量檢索。
  查證一手來源後確認**兩者實際不衝突**：Karpathy 在原文親自推薦 qmd（BM25／向量混合搜尋 + LLM 重排序），可見他反對的是「切塊當**知識層**」，不是「檢索當**定位工具**」——差別在檢索對象是原始文件碎片還是 wiki 整頁。
  處理：兩節都保留，加 callout 註記釐清層次差異並交叉引用 [[LLM-Wiki#可選：CLI 工具]]；「混合使用」改寫為「檢索層／定位／整頁」用語。**未刪任何一邊**。

**來源歸屬斷裂（已補）**

- `concepts/LLM-Wiki.md` 是全 wiki **唯一沒有「來源」區塊**的內容頁（23 頁中唯一）——推測為 2026-06-07「全文翻譯覆寫」時遺失。已補回兩個來源脈絡：Karpathy 原文 gist（正文翻譯）＋ 2026-07-05 網搜（生態節）。`source_count` 1 → 2、index 來源數同步。
  註：2026-07-05 lint 曾將 index 來源數由 2 改為 1「與 frontmatter 一致」——**當時對齊到錯的一邊**，實為 frontmatter 漏記。
- `concepts/RAG-vs-LLM-Wiki.md`「為什麼 Karpathy 反對 chunked RAG」節（2026-07-05 網搜產物）未列來源、未連查詢歸檔 → 補 [[2026-07-05-obsidian-llm-wiki]]，`source_count` 2 → 3、index 同步。

**斷鏈（已修）**

- **heading anchor 斷鏈 4 條**（首輪腳本切掉 `#` 後未驗，故漏掉）：
  `演算法策略.md` 的 `排序演算法#Merge Sort`／`#Quick Sort`、`搜尋演算法#Binary Search`，及 `資料結構總覽.md` 的 `排序演算法#Heap Sort`。
  實際標題含中文對照（如「Merge Sort（合併排序）」），錨點對不上 → 改為完整標題 + alias 保持顯示不變。

**格式（已修）**

- `concepts/知識庫架構設計.md` **4 處**與 `Ingest-工作流.md` 同款的單反引號圍欄 bug（其中一處包住目錄樹狀圖）→ 全改為三反引號。

**命名衝突（新發現）**

- `wiki/entities/KeyLogger-Server.md` 與 `工作專案/KeyLogger-Server.md` **完全同名**。`index.md` 的裸連結依 Obsidian 最短路徑規則會落到工作專案原始筆記（1 層 < 2 層）→ 已改用 `entities/` 路徑消歧義。
  （`entities/Ethan.md` 的同名連結因與 entity 頁同資料夾，解析正確，不受影響。）
- `wiki/concepts/LLM-Wiki.md` 與 `Clippings/AI 開發工具/llm-wiki.md` 僅大小寫不同，8 處裸 `[[LLM-Wiki]]` 靠大小寫精確匹配解析。目前運作正常但脆弱（改動任一檔名大小寫即失效）——**待決，未動**。
- `個人學習/Leecode/NeetCode Roadmap (...)` 有 `.canvas` 與 `.md` 同名兩份，`NeetCode-刷題路線.md` 來源連結指向不明確——**待決，未動**。

**工具備註（續）**

腳本第二個坑：PowerShell hashtable **預設大小寫不敏感**，`llm-wiki` 與 `LLM-Wiki` 視為同鍵，導致標題表被 Clippings 版覆蓋、誤報 anchor 斷鏈。另需排除 **inline code**（`` `[[範例]]` ``）否則說明文字裡的連結會被誤判為活斷鏈。最終腳本：排除 fenced + inline code、大小寫敏感、依「同資料夾 → 最短路徑」解析並標記撞名。

## [2026-07-21] lint | 健康檢查與修復

以 UTF-8-safe Python 腳本掃 26 個 wiki 內容頁 + 全 vault 原始區複點（處理跳脫管線 `\|` 與 `.canvas` 副檔名、排除 fenced/inline code、大小寫敏感）。距上次 lint（2026-07-16）原始區僅 2 檔異動 + 1 檔刪除，wiki/ 與 Clippings/ 內容無變動。

**已修（自動）**

- **索引補漏**：`Clippings/MCP vs API Why traditional APIs are failing AI agents.md`（2026-07-16 09:09 新增的 YouTube 剪藏，Google Cloud Tech）未登錄 index「待消化」清單、亦未 ingest → 補入清單（Clippings 2→3）。
- `index.md` updated → 2026-07-21。

**新發現斷鏈（待決，未動）**

- **來源檔遭刪**：`Clippings/職涯/SRE Engineer & AI Systems Developer.md` 於 2026-07-16 09:29 vault backup commit（2eb8fdd）中被刪 → `concepts/SRE-學習路徑.md`（L103）、`entities/Ethan.md` 各餘 1 條活斷鏈指向它。內容可自 git（2eb8fdd^）還原。等使用者裁示：還原原始檔 or 移除／改寫 wiki 來源引用。

**沿用待決（未動）**

- `entities/Andrej-Karpathy.md`：`[[范凱說AI]]`、`[[HC AI說人話]]` 無對應頁（外部自媒體）。自 2026-07-05 懸置。
- `entities/Claude-Code.md`：模型清單過時（Sonnet 4.6／GPT-5.5，非實際模型且未見於來源筆記）。自 2026-07-05 懸置。
- `CLAUDE.md`：「skills 檢查」「git 紀律」2 條與 `core_rules.md` 重複（AGENTS.md 無此問題）。自 2026-07-16 懸置。
- 命名脆弱：`LLM-Wiki` vs `llm-wiki`（僅大小寫）、`NeetCode Roadmap` .canvas/.md 同名、`KeyLogger-Server` 撞名（`Ethan.md` 裸連結依最短路徑解析到 `工作專案/` 原始檔而非 entity 頁）。

**檢查結果**

- 孤立頁 **0**（`KeyLogger-Server.md` 由 index 全路徑連入）。
- log.md 內斷鏈（`Best practice questions`、職涯 SRE 全路徑、`路徑\|別名`、`範例` 等）均為 append-only 歷史紀錄或語法範例，刻意保留。

## [2026-07-21] fix+ingest | 使用者裁示「全部修」——修完 6 項待決 + 消化全部待消化

承同日 lint，使用者回「全部修」。逐項處理：

**待決修復（1、4、5、6）**

- **斷鏈（1）**：自 git（`2eb8fdd^`）還原被誤刪的 `Clippings/職涯/SRE Engineer & AI Systems Developer.md` → `concepts/SRE-學習路徑.md`、`entities/Ethan.md` 兩條活斷鏈自動解除。
- **去連結化（4）**：`entities/Andrej-Karpathy.md` 的 `范凱說AI`、`HC AI說人話`（外部自媒體、無頁）由 wikilink 改純文字「『范凱說AI』與『HC AI說人話』等自媒體」。
- **過時內容（5）**：`entities/Claude-Code.md` 模型節。查來源 `插件安裝.md` 發現 `claude-sonnet-4-6`／`gemini-3.1-pro-low`／`gpt-5.5` 實為 DeepSeek 第三方後端 env 範例值，原頁誤distill成「Sonnet 4.6（預設）」「GPT-5.5 第三方模型」→ 改寫為「Claude 三層級 + 可用 `ANTHROPIC_DEFAULT_*_MODEL` 換第三方後端」，貼合來源。
- **規則重複（6）**：`CLAUDE.md` 刪去與 `core_rules.md` 重複的「skills 檢查」「git 紀律」兩條，改一句指回 `core_rules.md`，只留 CC 專屬補充。AGENTS.md 本就乾淨、未動。

**Ingest（2、3）：14 篇 → 新增 9 頁**

- 新概念頁 5：`LLM-入門`（Karpathy LLM 演講）、`MCP`（MCP vs API 剪藏）、`LLM-Wiki-搭建指南`（搭建指南）、`自製-Claude-Code-Skills`（總覽+安裝指南 2 篇合併）、`AI-產品開發工作流`（How I build 剪藏）。
- 新實體頁 4：`OpenCode`（oc-go-cc + MCP 配置 2 篇合併）、`Postfix-Manager`（系統+維護 2 篇合併）、`CORE-PULSE`、`Quartz-閱讀網站`。
- 更新交叉連結：`Ethan`（補 4 個專案/skill 連結）、`Claude-Code-Skills`（補自製技能節）、`Andrej-Karpathy`（補 `LLM-入門`）、`Claude-Code`（補 `OpenCode`/`MCP`）。
- **刻意不另建頁**：Codex 逆袭剪藏（推廣、知識稀薄，nugget 併入 `OpenCode`）；`2026-07 工作紀錄`（月度 meta-index，已指向各 wiki 頁）。

**隱私處理**：ingest 時未寫入來源中的個資與憑證（個人 email、API key/salt、預設密碼、本機路徑）——只提煉知識。

**驗證**：UTF-8 lint 腳本複掃，新頁 wikilink 全解析、無新增斷鏈、無孤立頁（`AI-產品開發工作流` 由 `自製-Claude-Code-Skills` 連入）。`index.md` 補登 9 頁、待消化歸零、updated 2026-07-21。

## [2026-07-21] fix | KeyLogger-Server 撞名消歧義（自動處理殘項）

承上，使用者要求自動處理唯一殘留的命名脆弱項。`entities/Ethan.md` 的裸連結 `[[KeyLogger-Server]]` 依 Obsidian 最短路徑會落到 `工作專案/KeyLogger-Server.md`（1 層）而非 entity 頁（2 層）→ 改為完整路徑 `[[wiki/entities/KeyLogger-Server|KeyLogger-Server]]`（條列用一般 `|`，非表格跳脫 `\|`）。效果：Ethan 確定連到 entity 頁，`entities/KeyLogger-Server.md` 由「僅 index 連入」升級為有內容頁入鏈。至此全 wiki 撞名項清空。
