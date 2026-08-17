---
title: Wiki 索引
updated: '2026-08-17'
---

# Wiki 索引

這是 AI 維護的知識提煉層目錄。所有 wiki 頁面由 AI 根據原始筆記生成與維護。
查詢時請先讀此索引定位相關頁面，再深入閱讀。

---

## 概念頁（concepts/）

### 知識管理與工具

| 頁面 | 摘要 | 來源數 |
|---|---|---|
| [[LLM-Wiki]] | Karpathy 提出的知識管理模式：AI 持續維護 Markdown Wiki，知識累積不重新檢索 | 2 |
| [[RAG-vs-LLM-Wiki]] | 兩種 AI 知識做法的核心差異比較 | 3 |
| [[知識庫架構設計]] | Karpathy 原版、范凱 5 層、HC 三層架構比較 | 2 |
| [[Ingest-工作流]] | 將新資料消化進 Wiki 的三步流程與確認機制 | 1 |
| [[Obsidian-插件]] | 這個 vault 常用的 Obsidian 插件清單與配置 | 1 |
| [[Obsidian-同步方案]] | 免費雲同步、手機端、導出等技巧比較 | 1 |
| [[Claude-Code-Skills]] | Claude Code 19+ 個 Skills 分類整理 | 2 |
| [[Claude-Code-Lazy-Packs]] | 12 個 Claude Code 懶人包技能清單與影片系列 | 1 |
| [[LLM-Wiki-搭建指南]] | 從零搭 LLM Wiki 的 6 步驟 + 5 大陷阱（含本 vault 案例） | 1 |
| [[自製-Claude-Code-Skills]] | 作者開源的 4 個 Agent Skill 與共同設計理念 | 2 |
| [[Matt-Pocock-Skills-for-Real-Engineers]] | 專為 AI Agent 設計的工程化流程技能庫（TDD、PRD、Grilling 等 SOP） | 1 |

### AI 與 LLM

| 頁面 | 摘要 | 來源數 |
|---|---|---|
| [[LLM-入門]] | 從心智模型理解 LLM：壓縮即理解、4 階段訓練管道、幻覺/CoT/LLM-as-OS | 1 |
| [[MCP]] | Model Context Protocol：架在 API 之上、讓模型自行發現與呼叫工具的語義層 | 1 |
| [[AI-產品開發工作流]] | 一人團隊用 AI 開發：superpowers 四階段 + 測試自動化 + Codex 交叉審查 | 1 |

### 計算機科學

| 頁面 | 摘要 | 來源數 |
|---|---|---|
| [[資料結構總覽]] | 線性+非線性結構分類、複雜度速查 | 5 |
| [[排序演算法]] | 9 種排序法比較（含複雜度對照表） | 1 |
| [[搜尋演算法]] | 線性/二分/內插搜尋法 + DFS/BFS + 費波那契優化 | 1 |
| [[演算法策略]] | 分治、DP、貪婪、回溯、分支界定 | 1 |
| [[NeetCode-刷題路線]] | 15+ 分類刷題路線、常用模式、覆蓋統計 | 3 |
| [[SRE-學習路徑]] | 6 階段 SRE 完整成長路線，從基礎建設到領導策略 | 1 |
| [[TCP-轉發與-portfwd]] | 4 種 TCP 轉發情境與 portfwd 工具庫重構（跨平台、附測試） | 1 |

### 語言學習

| 頁面 | 摘要 | 來源數 |
|---|---|---|
| [[多益文法體系]] | TOEIC 完整教材導覽：6 大類 29 章文法秒殺重點 + 閱讀/模擬地圖 | 37 |
| [[多益文法-進階難題]] | 800+ 高失分陷阱：假設倒裝、分詞構句、關代前置、一致/平行、否定倒裝 | 15 |

## 實體頁（entities/）

### 工具與平台

| 頁面 | 摘要 | 來源數 |
|---|---|---|
| [[Obsidian]] | 這個 vault 的主要平台，純 Markdown 筆記軟體 | 4 |
| [[Claude-Code]] | Anthropic 的命令列 AI 助理，本 vault 的主要 AI 介面 | 2 |
| [[Claudian]] | Obsidian 內建的 AI 助理插件 | 1 |
| [[OpenCode]] | 開源命令列 AI 助理：oc-go-cc 提供第三方模型、並掛載多個 MCP server | 2 |

### 專案（Ethan 作品）

| 頁面 | 摘要 | 來源數 |
|---|---|---|
| [[wiki/entities/KeyLogger-Server\|KeyLogger-Server]] | C++ Winsock Server-Client 鍵側錄程式（與 `工作專案/KeyLogger-Server.md` 同名，故用完整路徑消歧義） | 1 |
| [[Postfix-Manager]] | Django + Docker 自架郵件伺服器管理系統（Postfix/Dovecot/OpenDKIM + Fail2ban） | 2 |
| [[wiki/entities/CORE-PULSE\|CORE-PULSE]] | 個人品牌與 SRE/AI 展示平台（`19980803.xyz`，React 19 + Pages Functions + `/telemetry` + `/ask`） | 2 |
| [[my-note-web]] | 本 vault 的 Cloudflare Workers 雲端閱讀平台（Sharded KV + 零 Embedding RAG + Git 雙向同步） | 1 |
| [[Osaka-Web]] | 和風御朱印帳設計的離線旅遊儀表板（React + Cloudflare D1 + Zod 建置守門） | 1 |
| [[Osaka-Vault]] | AI 驅動的旅遊知識庫（雙層知識隔離 + MCP + R2 資產管線） | 1 |
| [[Quartz-閱讀網站]] | 公開閱讀站初期規劃（已被 [[my-note-web]] 取代） | 1 |
| [[wiki/entities/PA440-FW-data-configurator\|PA440-FW-data-configurator]] | Palo Alto PA-440 防火牆 CTI 威脅情資自動化注入與組態稽核工具 | 1 |

### 人物

| 頁面 | 摘要 | 來源數 |
|---|---|---|
| [[Andrej-Karpathy]] | LLM Wiki 概念的原作者，OpenAI 共同創辦人 | 2 |
| [[Ethan]] | SRE 工程師與 AI 系統開發者，個人技術檔案 | 1 |
| [[mathruffian-dot]] | Claude Code 懶人包系列作者 | 1 |
| [[Matt-Pocock]] | TypeScript 講師、AI Hero 創辦人與 mattpocock/skills 開發者 | 1 |

## 查詢歸檔（queries/）

| 頁面 | 摘要 | 日期 |
|---|---|---|
| [[2026-07-05-obsidian-llm-wiki]] | Obsidian × LLM Wiki 生態、外部實作對照與最佳實踐 | 2026-07-05 |
| [[2026-07-05-toeic-part5-秒殺sop]] | 多益 Part 5 秒殺解題 SOP：四步法／位置法／字尾／介連對比 | 2026-07-05 |
| [[2026-07-05-toeic-進階文法難題]] | 多益 800+ 進階難題彙整：依章節標題深挖的高失分陷阱 | 2026-07-05 |

---

## 原始資料位置

| 資料夾 | 說明 |
|---|---|
| Clippings/ | 網頁剪藏與外部文章 |
| Clippings/Conversations/ | 有價值的 AI 對話紀錄 |
| 個人學習/ | 個人學習筆記（Obsidian、SRE、工具研究、DSA） |
| 個人學習/Leecode/Solution/ | 52 篇 LeetCode 解題筆記 |
| 個人學習/資料結構-鐵人挑戰-35D/ | 35 天資料結構系列 |
| 個人學習/多益/ | 多益全套教材（75 篇） |
| 個人學習/LLM與AI/ | LLM 原理與入門筆記 |
| 工作專案/ | 工作與專案技術紀錄（19 篇） |
| 工作專案/作品集/ | 求職用專案簡報 5 份 + 總覽（2026-08 新增，含截圖脫敏規則） |
| 好工具推薦/ | 工具設定指南（OpenCode、oc-go-cc） |
| 日常/ | 生活紀錄（非知識層素材） |
| 靈感/ | 隨手靈感速記 |

## 待消化筆記

2026-08-17 lint + fix：wiki 內部零斷鏈、零錨點斷鏈、零孤立頁、零未登錄；使用者裁示「全部修」，消化 `工作專案/` 新內容並更新兩頁過時實體頁。

**近期已消化**
- 2026-08-17：新增 3 實體頁（[[my-note-web]]、[[Osaka-Web]]、[[Osaka-Vault]]）+ 1 概念頁（[[TCP-轉發與-portfwd]]）；改寫 [[wiki/entities/CORE-PULSE|CORE-PULSE]]（企鵝 widget→`/ask` 全頁）、[[Quartz-閱讀網站]]（已被 my-note-web 取代）；[[Postfix-Manager]] 補 fail2ban regex 漏洞與 DOCKER-USER 鏈修正；[[Ethan]] 補新專案連結
- 2026-07-24：[[wiki/entities/PA440-FW-data-configurator|PA440-FW-data-configurator]]（來源 `工作專案/PA440-FW-data-configurator`）、[[Matt-Pocock-Skills-for-Real-Engineers]]＋[[Matt-Pocock]]
- 2026-07-21：ingest 14 篇 → 新增 9 頁（5 概念 + 4 實體）

**刻意不另建頁（已審閱）**
- `工作專案/2026-08 工作紀錄.md` — 月度彙整；fail2ban regex 漏洞已併入 [[Postfix-Manager]]，其餘（CORE PULSE 品質補強、Bubble-Beam 寶可夢站、TOEIC Flow）屬各專案的維護紀錄，Bubble-Beam／TOEIC Flow 尚無獨立 wiki 頁但知識密度低，暫不建頁
- `工作專案/作品集/作品集總覽.md`、`Osaka_Vault_Portfolio_Presentation` 以外的簡報 — 求職簡報 meta-index，內容已提煉進各對應實體頁

**刻意不另建頁（已審閱）**
- [[Clippings/AI 開發工具/Codex 逆袭开始！国内畅玩 OpenAI Codex，对接自建 API 中转站完整教程]] — YouTube 描述＋推廣連結，知識稀薄；其「Codex 經自建 API 中轉站接入」的點已併入 [[OpenCode]] 的第三方模型主題。
- [[工作專案/2026-07 工作紀錄]] — 月度工作彙整，本身即指向各 wiki 頁的 meta-index，不重複建頁。

未列入：`日常/`、`靈感/`（生活與速記，非知識層素材）。
