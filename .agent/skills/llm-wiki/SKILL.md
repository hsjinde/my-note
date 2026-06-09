---
name: llm-wiki
description: Manage the LLM Wiki knowledge base in the vault. Ingest new sources into wiki/, query existing knowledge, run health checks (lint), and archive AI conversations. Use when the user says "ingest", "消化", "整理進 wiki", "查詢", "query", "健康檢查", "lint wiki", or mentions managing their wiki/ knowledge base.
---

# Skill: llm-wiki

這個 vault 使用 LLM Wiki 模式管理知識。AI 負責維護 `wiki/` 目錄下的提煉知識層，
原始筆記（Clippings/、個人學習/、工作專案/ 等）保持不變。

## 架構總覽

```
wiki/
├── index.md          ← 目錄（每次操作前先讀這裡）
├── log.md            ← 操作歷史（append-only）
├── concepts/         ← 概念頁：主題、技術、方法論
├── entities/         ← 實體頁：工具、框架、人物、專案
└── queries/          ← 歸檔的有價值問答
```

原始資料（不可修改）：
- `Clippings/` — 網頁剪藏、外部文章
- `Clippings/Conversations/` — 有價值的 AI 對話紀錄
- `個人學習/` — 個人學習筆記
- `工作專案/` — 工作與專案紀錄
- `資料結構-鐵人挑戰-35D/` — DSA 學習筆記

## 核心規則

1. **讀 index 再行動**：任何操作前先讀 `wiki/index.md` 掌握現有頁面。
2. **先給方案再執行**：每次 ingest/lint 前，先列出計畫讓使用者確認，收到確認後才執行。
3. **原始資料唯讀**：不修改 Clippings/、個人學習/、工作專案/ 等原始筆記。
4. **記錄操作**：每次操作結束後 append 一筆到 `wiki/log.md`。
5. **語言**：wiki 頁面用繁體中文，技術名詞可保留英文。
6. **wikilink**：wiki 頁面之間用 `[[頁面名稱]]` 互相連結，連結到原始筆記時也用 wikilink。
7. **frontmatter**：每個 wiki 頁面加入 title、tags、updated、source_count（引用的原始來源數）。

## 工作流一：Ingest（消化新資料）

### 觸發語言範例
- 「幫我消化這個剪藏」
- 「把 Clippings 裡的文章整理進 wiki」
- 「ingest [筆記名稱]」
- 「把今天的對話存進知識庫」

### 流程

**步驟 1：讀取**
- 讀 `wiki/index.md` 掌握現有 wiki 頁面
- 讀取目標原始筆記（由使用者指定，或掃描 Clippings/ 找未處理的）

**步驟 2：分析並提出方案（給使用者確認）**

列出以下計畫，等待使用者說「可以」或修改意見：

```
Ingest 方案：[筆記標題]

核心主題：[2-3 個關鍵詞]

建議操作：
1. 更新/建立 wiki/concepts/[概念].md — 新增 [什麼內容]
2. 更新/建立 wiki/entities/[工具名].md — 補充 [什麼資訊]
3. 在 wiki/index.md 新增索引項目

交叉連結：
- 與 [[現有wiki頁面]] 有關聯，建議補上連結

確認後開始執行？
```

**步驟 3：執行（使用者確認後）**
- 用 `obsidian` CLI 建立或更新 wiki 頁面
- 更新 `wiki/index.md` 的對應分類區塊
- Append 日誌到 `wiki/log.md`

### Wiki 頁面格式

**concepts/概念名稱.md**
```markdown
---
title: 概念名稱
tags: [分類標籤]
updated: YYYY-MM-DD
source_count: N
---

# 概念名稱

[2-3 句核心定義]

## 核心重點
- 要點一
- 要點二

## 與其他概念的關係
- 相關：[[其他概念]]
- 對比：[[另一個概念]]

## 來源
- [[原始筆記名稱]]（類型：web clipping / 學習筆記 / 對話）
```

**entities/工具名稱.md**
```markdown
---
title: 工具名稱
tags: [工具, 分類]
updated: YYYY-MM-DD
source_count: N
---

# 工具名稱

[一句話定義]

## 主要功能
- 功能一
- 功能二

## 使用場景
[在這個 vault 裡怎麼用]

## 相關工具
- [[其他工具]]

## 來源
- [[原始筆記名稱]]
```

## 工作流二：Query（知識查詢）

### 觸發語言範例
- 「查詢 SRE 相關的筆記」
- 「我想知道 RAG 和 LLM Wiki 的差別」
- 「整理一下我對 Obsidian 了解多少」

### 流程

1. 讀 `wiki/index.md` 找相關頁面
2. 讀相關 wiki 頁面（concepts/ 或 entities/）
3. 必要時再讀原始筆記補充細節
4. 給出回答，附帶引用來源（用 wikilink）
5. **判斷是否值得歸檔**：如果這個問答有知識價值，問使用者是否要存到 `wiki/queries/`
6. 如果發現 wiki 頁面有明顯缺失或錯誤連結，附帶提醒（不自動修改）

### 查詢歸檔格式（queries/）

```markdown
---
title: [問題摘要]
tags: [相關主題]
date: YYYY-MM-DD
---

# [問題]

## 回答
[完整回答]

## 引用來源
- [[wiki頁面]]
- [[原始筆記]]
```

## 工作流三：Lint（健康檢查）

### 觸發語言範例
- 「健康檢查 wiki」
- 「lint wiki」
- 「幫我看看 wiki 有沒有問題」

### 流程

**步驟 1：掃描**（逐一讀取所有 wiki 頁面）

**步驟 2：列出問題報告（給使用者確認要修什麼）**

```
Wiki 健康報告

孤立頁面（沒有其他頁面連結到這裡）：
- wiki/concepts/XXX.md

重複概念（建議合併）：
- wiki/concepts/A.md 和 wiki/concepts/B.md 內容高度重疊

缺少連結：
- wiki/concepts/C.md 提到了 D，但沒有加 wikilink

可能過時（原始筆記已有更新內容但 wiki 未反映）：
- wiki/entities/E.md — 對應的 [[原始筆記]] 有新資訊

請問要修復哪些？可以說「全部修」或指定項目。
```

**步驟 3：執行修復（使用者確認後）**
- 修復指定問題
- Append 日誌到 `wiki/log.md`

## 工作流四：存對話（AI 對話記錄）

### 觸發語言範例
- 「把這段對話存到筆記庫」
- 「把剛才討論的內容存起來」

### 流程

1. 整理對話的核心知識點（不是逐字存）
2. 用 obsidian CLI 建立 `Clippings/Conversations/YYYY-MM-DD-主題.md`
3. 提醒使用者：「已儲存，下次可以用 ingest 消化進 wiki」

### 對話筆記格式

```markdown
---
title: [主題]
date: YYYY-MM-DD
tags: [clippings, conversation, 相關主題]
---

# [主題]

## 討論背景
[1-2 句說明這段對話的起點]

## 核心結論
- 結論一
- 結論二

## 原始問答摘要
**Q**：[問題]
**A**：[重點回答]
```

## 工具使用優先順序

1. `obsidian` CLI（優先，因為會即時在 Obsidian 顯示）
2. 如果 obsidian CLI 不可用，退回直接讀寫檔案

## 注意事項

- 不要一次 ingest 大量筆記（每次 1-3 篇），確保品質
- wiki 頁面應該是提煉後的知識，不是原文複製
- 如果一篇原始筆記很長，可以分拆成多個 wiki 頁面
- 對話中如果使用者問了有趣問題，主動建議歸檔到 queries/