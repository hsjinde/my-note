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

## [2026-08-17] lint | 健康檢查：結構全綠，落差在「新內容未消化」

以 UTF-8-safe Python 腳本掃 38 個 wiki 內容頁 + 全 vault（366 個 md/canvas、144 個附件）。距上次 lint（2026-07-24）已 3 週餘。

**結構檢查：全綠**

斷鏈 0、heading anchor 0、孤立頁 0、未登錄 index 0。`log.md` 內 5 條斷鏈（`Best practice questions`、資料夾式連結、舊 `llm-wiki` 檔名）屬 append-only 歷史紀錄，依既有慣例刻意保留；ambiguous 僅餘 1 條（`entities/KeyLogger-Server.md` 來源指向 `工作專案/` 原始筆記，語義正確）。額外對 `工作專案/` 全區掃描：0 斷鏈。

**異動範圍**

自 07-24 起原始區異動**全集中於 `工作專案/`**（`個人學習/`、`好工具推薦/`、`Clippings/`、`日常/`、`靈感/` 零異動）。新增 8 篇約 1,400 行：`作品集/` 6 篇（08-14）、`2026-08 工作紀錄`（08-08）、`portfwd-TCP轉發工具重構筆記`（07-24 lint 後）。另有一批附件搬遷與截圖脫敏（`26946a6`、`ca43f33`）。

**已修（自動）——索引補漏**

- `index.md` 原始資料位置表：`工作專案/` 8 篇 → 19 篇；新增 `工作專案/作品集/` 一列
- `index.md`「待消化筆記」區塊：原停在 07-24「已消化」狀態，補上本輪 8 篇待消化清單
- `index.md` updated → 2026-08-17

**新發現：內容過時（待裁示，未動）**

- `entities/CORE-PULSE.md` **已被新筆記推翻**：頁面主體描述「右下角 SVG 企鵝浮動按鈕」，但 `2026-08 工作紀錄` 明載該浮動 widget 已於 7/18 移除、現況為 `/ask` 全頁；網址亦由 `core-pulse.19980803.xyz` 改為 `19980803.xyz`；新增的 `/telemetry`（Three.js 觀測台）、Pages Functions SEO middleware、a11y CI 契約等全未反映。
- `entities/Quartz-閱讀網站.md` **規劃已被實作取代**：頁面記「規劃中，尚未動工」、預計 Quartz 4 + GitHub Pages。實際上線的是 my-note-web（`note.19980803.xyz`，Cloudflare Workers + Hono + Sharded KV），技術路線與部署平台皆不同。

**未消化（待裁示）**

`作品集/` 5 份簡報涵蓋三個 wiki 尚無實體頁的專案（my-note-web、大阪旅券 OSAKA TRIP、Osaka Vault）；`2026-08 工作紀錄` 含 fail2ban jail regex 漏洞（587/465 埠 96% 攻擊靜默漏記）等可提煉知識；`portfwd` 為 204 行 TCP 轉發重構技術筆記。`entities/Ethan.md` 專案清單亦缺這批新專案。

**規則檔同步**：`CLAUDE.md`、`AGENTS.md` 仍為薄指標檔，無重複規則；`core_rules.md` 自 2026-07-05 未改動。

## [2026-08-17] fix+ingest | 使用者裁示「全部修」——消化 `工作專案/` 新內容 + 更新兩頁過時實體頁

承同日 lint，使用者回「全部修」。

**過時內容改寫（2 頁）**

- **[[wiki/entities/CORE-PULSE|CORE-PULSE]] 全面改寫**：舊頁主體是「右下角 SVG 企鵝浮動 widget」，據 `2026-08 工作紀錄` 該 widget 已於 7/18 移除、現為 `/ask` 全頁；網址 `core-pulse.19980803.xyz` → `19980803.xyz`。改寫涵蓋 `/telemetry`（Three.js 觀測台）、Pages Functions 建置期預編譯 wiki、不記 IP 限流、測試即契約（axe 焦點環／sitemap 同步 build gate）、Terminal Editorial 設計系統。`source_count` 1→2、index 摘要同步。
- **[[Quartz-閱讀網站]] 標記已取代**：舊頁記「規劃中，尚未動工／Quartz 4 + GitHub Pages」。實際落地為 [[my-note-web]]（Cloudflare Workers 自建），路線與平台皆不同。改寫為保留初期決策脈絡 + 明標「已被 my-note-web 取代」，並註明白名單發布原則被沿用；補相關連結。

**新增頁（4）**

- 實體頁 3：[[my-note-web]]（Sharded KV／零 Embedding RAG／Webhook+tarball 雙管線同步／Contents API 樂觀鎖／公開私有硬邊界）、[[Osaka-Web]]（御朱印帳設計系統／離線韌性狀態機／Zod 建置守門／GitHub API 行程回寫）、[[Osaka-Vault]]（雙層知識隔離／R2 ASCII key 正規化／MCP 整合／7 天 TTL 增量消化／狀態標記防幻覺）。
- 概念頁 1：[[TCP-轉發與-portfwd]]（4 種轉發情境、統一轉發原語、跨平台、28 項 pytest）。

**既有頁補強**

- [[Postfix-Manager]]：新增「兩個『有在跑卻看不到』的真實漏洞」節——(1) 封鎖規則掛 `INPUT` 被 Docker DNAT 繞過、改掛 `DOCKER-USER`；(2) jail regex `postfix/\w+\[` 匹配不到 `postfix/submission/smtpd`，587/465 埠 96% 攻擊靜默漏記，修為 `postfix/(?:\w+/)?\w+\[`（fail2ban-regex 13135→13972）。`source_count` 2→4，補作品集簡報與 `2026-08 工作紀錄` 兩個來源。
- [[Ethan]]：相關概念補 my-note-web／Osaka-Web／Osaka-Vault／TCP-轉發與-portfwd 四個連結，CORE-PULSE 描述由「AI 吉祥物」更新為「個人品牌與 SRE/AI 展示平台」。

**刻意不另建頁**：`2026-08 工作紀錄`（月度 meta-index，fail2ban nugget 已併入 Postfix-Manager；Bubble-Beam 寶可夢站、TOEIC Flow 知識密度低）、`作品集/` 5 份簡報＋總覽（求職 meta-index，內容已提煉進對應實體頁）。

**隱私處理**：Osaka Vault／Web 頁面未寫入出國日期、航班編號、住宿名稱（「本人何時不在家、住在哪」）；Postfix 未寫實際埠映射、主機商與封鎖網段——皆只提煉知識，貼合原始簡報的脫敏原則。

**驗證**：UTF-8 lint 腳本複掃全 vault → 斷鏈 0、錨點 0、孤立頁 0、未登錄 index 0；新增 4 頁全數有 wiki 內入鏈且已登錄。wiki 內容頁 38 → 42。

## [2026-07-24] fix | 使用者裁示「處理 1、2」——NeetCode 來源歧義、LLM-Wiki 大小寫撞名根除

承同日 lint，使用者回「處理 1、2」（第 3 項大阪旅遊剪藏依建議略過，使用者其後自行刪除該檔）。

**1. NeetCode 來源歧義（已修）**

`個人學習/Leecode/` 下 `NeetCode Roadmap (Full List Consolidated + Diagram)` 有 `.canvas`（5 KB，視覺 diagram）與 `.md`（44 KB，完整題目表格＋進度追蹤，且自身以 `![[…canvas]]` 內嵌該圖）兩份。查證確認 `.md` 為主體筆記、`.canvas` 為其附圖；概念頁 L70 早已明確引用 `.canvas` 作「Canvas 路線圖」，僅 L81「來源」用裸名（靠 Obsidian「無副檔名預設 `.md`」解析，語義本就正確但不自我說明）。

處理：L81 → `[[個人學習/Leecode/NeetCode Roadmap (Full List Consolidated + Diagram).md|NeetCode Roadmap（Full List 主體筆記）]]`（完整路徑＋副檔名），與 L70 的 `.canvas` 明確區隔；lint 的 ambiguous 警告由此消除。

**2. LLM-Wiki 大小寫撞名（已根除）**

調查：小寫 `[[llm-wiki]]` 全 vault **零引用**；該剪藏僅被 `concepts/LLM-Wiki.md` L108 以完整路徑引用 1 處。wiki 內裸 `[[LLM-Wiki]]` 實際約 20 處（非 log 舊記的 8 處），另有 3 處位於唯讀原始區（`工作專案/2026-07 工作紀錄`、`個人學習/` 2 篇）無法改。

使用者於三方案（維持＋記錄／改剪藏檔名根除／wiki 內全改完整路徑）中選**根除**，授權破例動一個原始區檔名：

- `Clippings/AI 開發工具/llm-wiki.md` → `llm-wiki-karpathy-gist.md`（**僅改檔名，內容未動**，維持原始區內容唯讀原則）
- 同步更新 `concepts/LLM-Wiki.md` L108 來源引用，並於該行註明原檔名與改名理由
- `log.md` 內 2 處舊引用（L169、L304）屬 append-only 歷史紀錄，依既有慣例**刻意保留不改寫**

效果：vault 內 `LLM-Wiki` basename 唯一，所有 `[[LLM-Wiki]]`（含唯讀原始區那 3 處）不再依賴 Obsidian「大小寫完全相符優先」的隱性行為，撞名根除。自 2026-07-16 懸置的待決項結案。

**工具備註（新記一筆，與既有編碼坑並列）**

本輪 Grep 工具連續兩次對 `llm-wiki` 回報「零結果」，與 L108 實際存在明顯矛盾；改用 Bash `grep -rn` 才取得正確清單（3 處引用）。**改名這類牽一髮動全身的操作前，務必交叉驗證引用清單，勿單憑一次搜尋結果**——否則會漏改引用而製造斷鏈。

**index 補正**：移除「待裁示」的大阪旅遊剪藏一條——該檔已於 16:52 的 vault backup（`f037cef`）中由使用者刪除，不再是待決事項。

**驗證**：重跑腳本 → 斷鏈 0、heading anchor 0、孤立頁 0、未登錄 index 0；ambiguous 僅餘 1 條刻意保留（`entities/KeyLogger-Server.md` 的來源指向 `工作專案/` 原始筆記，語義正確）。全 vault md/canvas 由 236 → 235，經 `git log` 查證為使用者刪除大阪剪藏所致，非本次操作誤刪。

## [2026-07-24] lint | 健康檢查與撞名交叉引用修復

以 UTF-8-safe Python 腳本掃 38 個 wiki 內容頁 + 全 vault（236 個 md/canvas）複點。距上次 lint（2026-07-21）新增 2 筆 ingest（[[wiki/entities/PA440-FW-data-configurator|PA440]]、[[Matt-Pocock]]／[[Matt-Pocock-Skills-for-Real-Engineers]]），本輪針對其引入的撞名問題修復。

**腳本工具坑（新記一筆）**

首版腳本把表格跳脫管線 `\|` 誤還原為字面 `|` 再 split，使 `[[path\|alias]]` 的 target 含管線 → 誤報 39 條斷鏈（多益 hub 37 條 + index 的 KeyLogger／PA440 完整路徑 2 條）全為假陽性。修正：`\|` 應直接視為被跳脫的別名分隔符（還原成 `|` 後取前段）。沿用舊備註：排除 fenced/inline code、大小寫敏感、`.canvas` 副檔名、最短路徑解析。

**已修（自動）——撞名交叉引用統一化**

根因：`工作專案/` 下的 `CORE-PULSE.md`、`KeyLogger-Server.md`、`PA440-FW-data-configurator.md` 與 wiki entity 同名，裸 `[[名稱]]` 依 Obsidian 最短路徑一律落到工作專案原始筆記，連不到 entity 頁。07-21 只點修了 Ethan→KeyLogger 一條，07-21／07-24 新建的 entity 頁又大量沿用裸連結。本輪把「相關／其他專案／清單」導覽用的交叉引用全數改完整路徑（表格用 `\|`、條列用 `|`）：

- `index.md`：`[[CORE-PULSE]]` → 完整路徑（解除 CORE-PULSE 孤立）
- `entities/Ethan.md`：`[[CORE-PULSE]]` → 完整路徑；並補 `[[PA440-FW-data-configurator]]` 專案連結（解除 PA440 僅 index 連入）
- `entities/CORE-PULSE.md`、`Postfix-Manager.md`、`Quartz-閱讀網站.md`：其「其他專案」清單內的 `[[KeyLogger-Server]]`／`[[CORE-PULSE]]` → 完整路徑
- `entities/PA440-FW-data-configurator.md`：L30／L34 與自身同名的裸連結明確化為 `[[工作專案/PA440-FW-data-configurator|…]]`（本就指原始筆記，消除裸名脆弱）

**index 補漏**：`updated` → 2026-07-24；「待消化」區塊更新（反映 07-24 已 ingest PA440／Matt-Pocock、標記大阪旅遊剪藏待裁示）。

**保留（目標正確，非問題）**：entity 頁「來源」區塊的 `[[KeyLogger-Server]]`（指向 `工作專案/` 原始筆記為正確語義，故不改）。

**語義層（矛盾/過時）**：本次 delta 僅 07-24 兩頁。核對新 concept 頁 [[Matt-Pocock-Skills-for-Real-Engineers]] 與相鄰兄弟頁（[[AI-產品開發工作流]]／[[自製-Claude-Code-Skills]]／[[Claude-Code-Skills]]）——四者同屬 AI Agent 工程化但定位互異（Matt 第三方技能庫／superpowers 方法論／Ethan 自製技能／CC skills 機制），且已互相 wikilink，無重複或矛盾。順手補 [[Matt-Pocock-Skills-for-Real-Engineers]]、[[Matt-Pocock]] 兩頁缺漏的 `source_count: 1`（對齊 index 來源數）；其餘 schema 漂移（多出 `type` 欄位、tags 多行、`updated` 加引號）屬 07-24 新頁風格差異，未強制統一，留待日後定慣例。

**規則檔同步**：`CLAUDE.md`、`AGENTS.md` 均仍為指向 `core_rules.md` 的薄指標檔，無重複規則；`core_rules.md` 本次未改。

**驗證**：修後重跑腳本 → 斷鏈 0、heading anchor 0、孤立頁 0、未登錄 index 0；ambiguous 僅餘 2 條刻意保留（KeyLogger 來源、NeetCode Roadmap `.canvas`／`.md`）。

**待決（未動，待裁示）**

- `NeetCode-刷題路線.md` 來源 `[[NeetCode Roadmap (...)]]` 有 `.canvas`／`.md` 同名，裸連結解析到 `.md`：來源要指哪個。
- `LLM-Wiki`（wiki）vs `llm-wiki`（Clippings）僅大小寫之差，8 處裸連結靠大小寫精確匹配解析，脆弱：是否永久消歧義。自 2026-07-16 懸置。
- `Clippings/大阪…天神橋筋商店街…`（旅遊影片剪藏）：是否 ingest（建議略過，屬旅遊／日常素材）。

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

## [2026-08-18] merge | 兩台機器的同批 ingest 分歧收斂 + 埠號脫敏

本機工作區留著 08-14 未提交的作品集 ingest 草稿，遠端 `6cca725`（08-17）已有另一輪針對同一批來源、更完整的版本並推上去，兩者九個檔案全數衝突。處置：**以遠端為基底**（08-17 版事實較新，例如把企鵝 widget 從「是否已下線未確認」改為明確的已下線改 `/ask` 全頁），只前移本機獨有的內容，舊 commit 保留在本機分支 `wip/ingest-0814` 備查。

**前移（1 頁）**

- `concepts/設計系統實踐.md` — 三套自建設計系統（Terminal Editorial／書房紙頁／和風手帳）的共通硬約束。遠端該輪未建此頁，內容與 08-17 版三個專案頁的敘述逐條核對過、無矛盾。`index.md` 補登，並自 [[Ethan]]、[[wiki/entities/CORE-PULSE|CORE-PULSE]]、[[my-note-web]]、[[Osaka-Web]] 各補一條入鏈避免孤立頁。

**隱私修正**

- `entities/Postfix-Manager.md` 遠端版仍寫死 Nginx SSL Stream 的對外埠號，這正是作品集簡報刻意脫敏的項目。改為「未被主機商封鎖的高位埠」概括描述，頁首補 note 標明本頁不記具體座標（埠、內網位址、封鎖網段、主機商、OS 版本）——`hsjinde/my-note` 是 public repo，`wiki/` 對 GitHub 無遮蔽效果。**注意：從 HEAD 移除不等於從 git 歷史移除，舊 commit 仍查得到該數字。**

**未動**：`concepts/TCP-轉發與-portfwd.md` 已檢查無敏感座標；`工作專案/` 唯讀原始區未改。
