---
title: 搭建屬於自己的 LLM Wiki - Karpathy 模式實作指南
tags: [個人學習, LLM-Wiki, 知識管理, Obsidian, Claude-Code, 教學]
date: 2026-06-22
source: Karpathy LLM Wiki Gist + 本 Vault 實作經驗
---

# 搭建屬於自己的 LLM Wiki：Karpathy 模式實作指南

> 本文教你從零打造一個「AI 幫你維護的個人知識庫」——依 [[Andrej-Karpathy]] 提出的 [[LLM-Wiki]] 模式，並以本 Vault 的實際落地作為案例。
>
> 適合讀者：想把剪藏、學習筆記、AI 對話沉澱成長期知識資產的人。

## 0. 一句話定位

**LLM Wiki ＝ 你丟原始資料 ＋ AI 持續把它們提煉、交叉引用、維護成一份互相連結的 Markdown Wiki。**

關鍵差異（vs 多數人熟悉的 RAG）：

| | RAG | LLM Wiki |
|---|---|---|
| 知識累積 | 每次重新檢索 | 持久保存、持續累積 |
| 維護 | 自動索引 | AI 主動整理 + 交叉引用 |
| 平台綁定 | 綁 embedding model | 純 Markdown，任意模型可讀 |

> [!note] 詳細比較見 [[RAG-vs-LLM-Wiki]]。這裡只記住：LLM Wiki 的本質是「**知識被編譯一次，然後保持更新**」。

## 1. 核心架構：3 層 + 2 個特殊檔

```
┌────────────────────────────────────────────┐
│  Schema（AGENTS.md / CLAUDE.md）          │  ← 告訴 AI 怎麼維護 Wiki
├────────────────────────────────────────────┤
│  Wiki /（AI 擁有、AI 寫）                  │  ← 提煉後的知識層
│    ├── index.md    ← 內容目錄              │
│    ├── log.md      ← 操作歷史              │
│    ├── concepts/   ← 概念頁                │
│    ├── entities/   ← 實體頁                │
│    └── queries/    ← 問答歸檔              │
├────────────────────────────────────────────┤
│  Raw Sources（你擁有、唯讀）               │  ← 不可改的原始資料
│    Clippings/、個人學習/、工作專案/ ...    │
└────────────────────────────────────────────┘
```

### 三層職責

| 層 | 誰寫 | 內容 |
|---|---|---|
| **Raw Sources** | 你 | 剪藏、筆記、對話紀錄。**不可修改** |
| **Wiki** | AI | 摘要、概念頁、實體頁、比較頁、index、log |
| **Schema** | 你 + AI 共演化 | 告訴 AI Wiki 的結構慣例與工作流程 |

### 兩個特殊檔

- **`index.md`**：內容導向目錄。AI 每次操作前先讀這裡定位。中等規模（~100 來源、數百頁）就夠用，不需 RAG 基礎設施。
- **`log.md`**：時間順序操作記錄。append-only，格式固定前綴（`## [YYYY-MM-DD] op | 說明`），可用 `grep` 快速回顧。

> [!important] 這兩個檔是 Wiki 的導航系統。少了它們，AI 每次都要重新掃描整個 Wiki 才知道現狀。

## 2. 工具選擇：為什麼是 Obsidian + Claude Code

Karpathy 原話：**「Obsidian 是 IDE；LLM 是程式設計師；Wiki 是程式碼庫。」**

| 工具 | 角色 | 為什麼選它 |
|---|---|---|
| **Obsidian** | IDE / 瀏覽器 | 純 Markdown、本地檔案、圖譜視圖、wikilink、外掛生態 |
| **Claude Code** | 程式設計師 | 命令列 AI、可讀寫檔案、可執行 shell、有 Skills 機制封裝工作流 |

替代組合：
- **Obsidian + Cursor / Codex / OpenCode**：任何能讀寫檔案的 AI agent 都行
- **VS Code + Claude Code**：犧牲 Obsidian 的圖譜視圖，但寫作體驗更程式導向
- **純檔案系統 + 任意 LLM**：極簡，但沒有即時視覺化

> [!tip] 平台不綁定是 LLM Wiki 的核心優勢。Wiki 是純 Markdown，換 AI 模型不影響知識。

## 3. 從零搭建：6 個步驟

### Step 1｜建立 Obsidian Vault

```bash
# 安裝 Obsidian 後
File → New vault → 命名（例：my-wiki）
```

**關鍵設定**：
- Settings → Files and links → Attachment folder path → 設成 `assets/`
- Settings → Hotkeys → 綁定「Download attachments for current file」（剪藏後可一鍵下載圖片到本地）

### Step 2｜規劃目錄結構

最小可用結構：

```
my-wiki/
├── Clippings/              ← 網頁剪藏（唯讀）
├── 個人學習/               ← 學習筆記（唯讀）
├── 工作專案/               ← 工作紀錄（唯讀）
├── wiki/                   ← AI 維護層
│   ├── index.md
│   ├── log.md
│   ├── concepts/
│   ├── entities/
│   └── queries/
├── assets/                 ← 圖片附件
├── AGENTS.md               ← Schema 檔（給 AI agent 讀）
├── CLAUDE.md               ← Claude Code 專用 schema
└── core_rules.md           ← 共用規則
```

> [!note] 唯讀層的資料夾名稱可自訂。重點是**保留你既有的分類邏輯**，不要為了系統重組。

### Step 3｜撰寫 Schema 檔（核心）

Schema 是 LLM Wiki 的靈魂——它讓 AI 從「通用聊天機器人」變成「有紀律的 Wiki 維護者」。

**`core_rules.md` 範本**（可複製修改）：

```markdown
# Core Rules

本 Vault 為 LLM Wiki 知識管理系統：AI 提煉原始筆記 into wiki/ 知識層，
原始資料保持唯讀。

## 工具與協作
- 筆記操作優先序：obsidian-cli (Skill) ➔ MCP ➔ 檔案直接讀寫
- Wiki 操作：觸發關鍵字時優先調用 /llm-wiki skill
- Git：禁止主動 commit/push/reset，除非明確要求

## 目錄與知識架構
- wiki/：AI 維護的知識層。操作前必讀 wiki/index.md，操作後記錄至 wiki/log.md
- 唯讀原始區：Clippings/、個人學習/、工作專案/。AI 應提煉而非直接大改

## LLM Wiki 觸發關鍵字
- Ingest / 消化 / 整理進 wiki：將原始筆記提煉進知識庫
- 查詢 / query / 搜尋知識：從 wiki 查詢
- 健康檢查 / lint wiki：檢查 wiki 結構問題
- 存對話 / 保存討論：將 AI 對話存為 Clippings/Conversations/

## 寫作與格式規範
- 極致精簡：繁體中文回覆。短段落、精簡條列
- 語法：相容 Obsidian 格式。內部連結用 [[Wikilinks]]
- 內容提煉：重點在於「提煉、重組、連結知識」，而非單純美化排版

## 互動與安全
- 防呆確認：需求不明確時，先問 1 個關鍵問題，絕不自行大量假設
- 隱私安全：不保存、不上傳、不擴散敏感個資與憑證
```

**`AGENTS.md` 範本**（給 Codex / OpenCode 用）：

```markdown
# AGENTS.md

請遵守共用規則：[core_rules.md](./core_rules.md)

## Agent 補充
- 以 core_rules.md 作為主要規則來源，不要在本檔重複維護相同內容
- 跨檔修改前先確認影響範圍，避免覆蓋既有筆記
```

**`CLAUDE.md` 範本**（給 Claude Code 用）：

```markdown
# CLAUDE.md

請遵守共用規則：[core_rules.md](./core_rules.md)

## Claude Code 補充
- 優先使用本 vault 既有的 .claude/skills/，避免重複建立
- 修改筆記或設定前，先理解目前檔案結構與既有內容
- 除非使用者明確要求，不要主動 commit、push 或執行破壞性 git 操作
```

> [!important] Schema 是「你 + AI 共演化」的文件。用一段時間後，把學到的教訓寫回去。

### Step 4｜建立 `wiki/` 骨架

**`wiki/index.md`**（初始狀態）：

```markdown
---
title: Wiki 索引
updated: '2026-06-22'
---

# Wiki 索引

這是 AI 維護的知識提煉層目錄。查詢時請先讀此索引定位相關頁面。

## 概念頁（concepts/）

| 頁面 | 摘要 | 來源數 |
|---|---|---|
| _(待 ingest 後填入)_ | | |

## 實體頁（entities/）

| 頁面 | 摘要 | 來源數 |
|---|---|---|
| _(待 ingest 後填入)_ | | |

## 查詢歸檔（queries/）

> 尚未建立。有價值的問答會自動歸檔於此。

## 待消化筆記

> 全部消化完畢。
```

**`wiki/log.md`**（初始狀態）：

```markdown
---
title: Wiki 操作日誌
---

# Wiki 操作日誌

記錄每次對 wiki 的重要操作，格式：## [日期] 操作類型 | 說明

---

## [2026-06-22] init | Wiki 知識庫初始化

- 建立 wiki/ 目錄結構：index.md、log.md、concepts/、entities/、queries/
```

### Step 5｜安裝 `llm-wiki` skill

Skill 是封裝好的工作流，告訴 AI 如何執行 ingest / query / lint。

建立 `.claude/skills/llm-wiki/SKILL.md`，內容涵蓋：

- **Ingest 工作流**：讀 → 提方案 → 等確認 → 執行 → 更新 index/log
- **Query 工作流**：讀 index → 讀相關頁 → 給答案帶引用 → 詢問是否歸檔到 queries/
- **Lint 工作流**：掃描 → 列問題報告 → 等確認 → 修復 → 記 log
- **Wiki 頁格式**：concepts/、entities/、queries/ 各自的 frontmatter 與區塊結構

> [!tip] 完整 skill 範本可參考 [Karpathy gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 或本 Vault 的 `.claude/skills/llm-wiki/SKILL.md`。

### Step 6｜第一次 Ingest

跟 AI agent 說：

```
幫我消化 Clippings/ 裡的第一篇文章，整理進 wiki
```

AI 會：
1. 讀 `wiki/index.md` 掌握現有頁面
2. 讀目標文章
3. **提方案給你確認**（核心防呆）：

```
Ingest 方案：[文章標題]

核心主題：[2-3 個關鍵詞]

建議操作：
1. 建立 wiki/concepts/[概念].md — 新增 [什麼內容]
2. 建立 wiki/entities/[作者].md — 補充 [什麼資訊]
3. 在 wiki/index.md 新增索引項目

交叉連結：
- 與 [[現有 wiki 頁]] 有關聯

確認後開始執行？
```

4. 你說「可以」→ AI 寫頁面、更新 index、append log

> [!important] 「先提方案再執行」是 LLM Wiki 不走歪的關鍵。全自動容易分類錯、覆蓋既有頁面。10 秒確認勝過事後補救。

## 4. 三大工作流

### Ingest（消化新資料）

```
你丟新來源 → AI 讀 index → AI 讀來源 → AI 提方案
       → 你確認 → AI 寫/更新 wiki 頁 → AI 更新 index → AI append log
```

- 一次 1-3 篇，確保品質
- 單一來源可能觸發 10-15 個 wiki 頁的更新

### Query（查詢知識）

```
你問問題 → AI 讀 index 定位 → AI 讀相關頁 → AI 綜合答案帶引用
       → （有價值則）詢問是否歸檔到 queries/
```

> [!tip] 好的查詢答案應該歸檔回 Wiki 成為新頁面。這樣探索會像 ingest 一樣累積。

### Lint（健康檢查）

定期執行，找：
- 孤立頁面（沒人連結到）
- 矛盾頁面（新來源推翻舊觀點但沒更新）
- 缺漏交叉引用
- 被提及但沒獨立頁的重要概念

AI 列出問題報告 → 你指定修哪些 → AI 修 → 記 log

## 5. 本 Vault 實作案例

這個 Vault 本身就是一個 running example。以下是關鍵片段。

### 實際目錄結構

```
my-note/
├── Clippings/                    ← 7 篇網頁剪藏
│   └── Conversations/            ← AI 對話存檔
├── 個人學習/                     ← 學習筆記
│   ├── Leecode/Solution/         ← 52 篇解題
│   ├── 資料結構-鐵人挑戰-35D/    ← 35 天系列
│   └── obsidian相關筆記/
├── 工作專案/                     ← 工作紀錄
├── wiki/                         ← AI 維護層
│   ├── index.md
│   ├── log.md
│   ├── concepts/  (10 頁)
│   ├── entities/  (5 頁)
│   └── queries/   (待建立)
├── .claude/skills/llm-wiki/      ← 封裝的工作流
├── AGENTS.md
├── CLAUDE.md
└── core_rules.md
```

### Schema 檔分工

- `core_rules.md`：共用規則（30 行，極精簡）
- `AGENTS.md`：給 Codex/OpenCode 的補充
- `CLAUDE.md`：給 Claude Code 的補充

> [!note] 兩個 schema 檔都引用 `core_rules.md`，避免規則重複維護。

### `log.md` 真實片段

```markdown
## [2026-06-04] ingest | 第一批：obsidian相關筆記 4 篇

來源：
- [[個人學習/obsidian相關筆記/3 層架構打造個人 AI 大腦...]]
- [[個人學習/obsidian相關筆記/Karpathy 的 LLM Wiki 火了...]]
- ...

建立概念頁 6 個：
- wiki/concepts/LLM-Wiki.md
- wiki/concepts/RAG-vs-LLM-Wiki.md
- ...

建立實體頁 4 個：
- wiki/entities/Obsidian.md
- wiki/entities/Claude-Code.md
- ...

更新：wiki/index.md
```

> [!tip] 固定前綴 `## [日期] op | 說明`，可用 `grep "^## \[" wiki/log.md | tail -5` 快速看最近 5 筆操作。

### `index.md` 真實片段

```markdown
## 概念頁（concepts/）

### 知識管理與工具

| 頁面 | 摘要 | 來源數 |
|---|---|---|
| [[LLM-Wiki]] | Karpathy 提出的知識管理模式 | 2 |
| [[RAG-vs-LLM-Wiki]] | 兩種 AI 知識做法的核心差異 | 2 |
| [[知識庫架構設計]] | Karpathy 原版、范凱 5 層、HC 三層比較 | 2 |
```

### 實際 ingest 軌跡

本 Vault 在 4 天內分 6 批消化完所有原始資料：

1. 第一批：obsidian相關筆記 4 篇 → 6 概念頁 + 4 實體頁
2. 第二批：SRE 相關 2 篇 → 1 概念頁 + 1 實體頁
3. 第三批：Clippings 3 篇 → 2 概念頁 + 1 實體頁
4. 第四批：資料結構鐵人挑戰 35 篇 → 4 概念頁（不逐篇 ingest）
5. 第五批：NeetCode 刷題路線 + 52 篇解題 → 1 概念頁
6. 第六批：工作專案 KeyLogger → 1 實體頁

> [!important] 觀察：52 篇 LeetCode 解題只提煉成 1 個路線頁。Wiki 是「知識地圖」不是「全集」——細節留在 Raw 層。

## 6. 進階與規模化

### 當 Wiki 長到 Context Window 不夠用

**解法 1：本地搜尋引擎**
- [qmd](https://github.com/tobi/qmd)：BM25 + 向量混合搜尋 + LLM 重排，本機跑
- 同時有 CLI（AI 可 shell out）和 MCP server（AI 當原生工具用）

**解法 2：在 LLM Wiki 上層疊 RAG**
- Wiki 頁當 chunk → 向量索引 → 查詢時先檢索相關頁 → AI 讀頁面再回答
- 純 Wiki 與純 RAG 的混合體

### 自動化與工具補強

- **Dataview（Obsidian 插件）**：對 frontmatter 跑查詢，生成動態表格
- **Canvas**：把 Wiki 頁面拖進 Obsidian Canvas 做視覺化關聯圖
- **Git**：版本控制 Wiki 變化（但讓 AI 不要主動 push）

### 平台不綁定原則

- Wiki 是純 Markdown → 換 AI 模型不影響
- 換 Obsidian → 只要把資料夾搬走即可
- 換作業系統 → 路徑調整就好

> [!warning] 避免把知識寫進某個工具的專屬格式（如 Notion database、Roam block ref）。純 Markdown + wikilink 是最可攜的。

## 7. 常見陷阱

### 陷阱 1：全自動 ingest 容易分類錯

**症狀**：AI 把新文章歸到錯誤的概念頁，或覆蓋掉既有頁面。

**解方**：強制「先提方案、人類 10 秒確認再執行」。這是 `llm-wiki` skill 的核心設計。

### 陷阱 2：不 Lint 會髒

**症狀**：3 個月後出現孤立頁、矛盾陳述、沒被連結的重要概念。

**解方**：每月跑一次 lint。讓 AI 列問題報告，你指定修哪些。

### 陷阱 3：AI 對話沒存下來就消失

**症狀**：跟 AI 討論出有價值的洞見，關掉視窗就沒了。

**解方**：用「存對話」關鍵字觸發 → AI 整理成 `Clippings/Conversations/YYYY-MM-DD-主題.md` → 之後再 ingest 進 wiki。

### 陷阱 4：把 Wiki 當 Raw 全集

**症狀**：逐篇 ingest 52 篇解題筆記，Wiki 膨脹又沒價值。

**解方**：Wiki 是「知識地圖」，不是「全集」。大量同質資料提煉成 1 個路線頁，細節留在 Raw 層。

### 陷阱 5：Schema 太複雜

**症狀**：AGENTS.md 寫到 500 行，AI 反而不遵守。

**解方**：極致精簡。本 Vault 的 `core_rules.md` 只有 30 行。規則越少，AI 越能遵守。

## 8. 延伸閱讀

### Karpathy 原始素材

- [LLM Wiki Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — 原版概念文件
- [[LLM-Wiki]] — 本 Vault 對此概念的完整翻譯與整理
- [[Andrej-Karpathy]] — 作者簡介

### 架構演進

- [[知識庫架構設計]] — Karpathy 原版、范凱 5 層、HC 三層架構比較
- [[RAG-vs-LLM-Wiki]] — 與 RAG 的核心差異
- [[Ingest-工作流]] — Ingest 的詳細流程

### 本 Vault 工具鏈

- [[Obsidian]] — 主要平台
- [[Claude-Code]] — AI 介面
- [[Claude-Code-Skills]] — Skills 機制封裝工作流

---

> 想消化這篇進 wiki 知識庫，跟我說一聲（會觸發 `/llm-wiki` ingest）。
