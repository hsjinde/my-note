---
title: LLM Wiki 搭建指南
tags: [LLM-Wiki, 知識管理, Obsidian, Claude-Code, 教學]
updated: 2026-07-21
source_count: 1
---

# LLM Wiki 搭建指南

從零打造「AI 幫你維護的個人知識庫」的實作步驟，依 [[Andrej-Karpathy]] 的 [[LLM-Wiki]] 模式，以本 Vault 為 running example。概念本身見 [[LLM-Wiki]]，架構比較見 [[知識庫架構設計]]，本頁專注在**怎麼搭 + 常見陷阱**。

## 核心架構：3 層 + 2 個特殊檔

| 層 | 誰寫 | 內容 |
|---|---|---|
| **Schema**（`core_rules.md` / `AGENTS.md` / `CLAUDE.md`） | 你 + AI 共演化 | 告訴 AI 怎麼維護 Wiki |
| **Wiki/**（`concepts/`、`entities/`、`queries/`） | AI 擁有、AI 寫 | 提煉後的知識層 |
| **Raw Sources**（`Clippings/`、`個人學習/`…） | 你擁有、唯讀 | 不可改的原始資料 |

兩個特殊檔是導航系統：**`index.md`**（內容目錄，AI 每次操作前先讀）＋ **`log.md`**（append-only 操作歷史，固定前綴 `## [日期] op | 說明`，可 grep）。少了它們，AI 每次都要重掃整個 Wiki。

## 工具選擇

Karpathy 原話：**「Obsidian 是 IDE；LLM 是程式設計師；Wiki 是程式碼庫。」**

- **[[Obsidian]]**：純 Markdown、本地檔案、圖譜、wikilink、外掛生態。
- **[[Claude-Code]]**：命令列 AI，可讀寫檔案、跑 shell、有 [[Claude-Code-Skills]] 封裝工作流。
- 替代：Obsidian + [[OpenCode]] / Cursor / Codex，或純檔案系統 + 任意 LLM。平台不綁定是核心優勢（Wiki 是純 Markdown，換模型不影響知識）。

## 從零搭建 6 步

1. **建 Obsidian Vault**：附件路徑設 `assets/`。
2. **規劃目錄**：保留你既有的分類邏輯（唯讀原始區），另開 `wiki/`。
3. **寫 Schema 檔**（靈魂）：`core_rules.md` 定規則、觸發關鍵字、寫作規範；`AGENTS.md` / `CLAUDE.md` 只引用它、不重複維護。
4. **建 `wiki/` 骨架**：空的 `index.md` + `log.md`（init 紀錄）。
5. **裝 `llm-wiki` skill**：把 ingest / query / lint 工作流封裝進 `.claude/skills/`。
6. **第一次 Ingest**：AI 讀 index → 讀來源 → **提方案給你確認** → 寫頁、更新 index、append log。

## 三大工作流

- **Ingest**：你丟來源 → AI 提方案 → 你確認 → AI 寫/更新頁 + index + log。一次 1–3 篇保品質；單一來源可能觸發 10–15 頁更新。詳見 [[Ingest-工作流]]。
- **Query**：你問 → AI 讀 index 定位 → 綜合答案帶引用 →（有價值則）歸檔到 `queries/`。
- **Lint**：定期找孤立頁／矛盾／缺漏交叉引用／被提及但無頁的概念 → 列報告 → 你指定修哪些 → 記 log。

## 5 大陷阱

| 陷阱 | 症狀 | 解方 |
|---|---|---|
| 全自動 ingest | 分類錯、覆蓋既有頁 | 強制「先提方案、人類 10 秒確認再執行」 |
| 不 lint 會髒 | 孤立頁、矛盾、沒被連的重要概念 | 每月跑一次 lint |
| 對話沒存就消失 | 有價值的討論關窗即失 | 「存對話」→ `Clippings/Conversations/` → 再 ingest |
| 把 Wiki 當全集 | 逐篇 ingest 52 篇解題，膨脹又沒價值 | Wiki 是「知識地圖」不是全集；同質資料提煉成 1 頁，細節留 Raw |
| Schema 太複雜 | 規則寫到 500 行，AI 反而不遵守 | 極致精簡（本 vault `core_rules.md` 僅約 35 行） |

## 規模化

- **Wiki 大到塞不進 context**：本地搜尋引擎（qmd：BM25＋向量混合＋LLM 重排），或在 Wiki 頁上疊 RAG（頁當 chunk 定位）——見 [[RAG-vs-LLM-Wiki]] 對「檢索當定位工具」的釐清。
- **可攜性原則**：避免寫進工具專屬格式（Notion database、Roam block ref）；純 Markdown + wikilink 最可攜。

## 相關概念

- [[LLM-Wiki]] — 概念本體與 Karpathy 原始 gist
- [[知識庫架構設計]] — Karpathy 原版 / 范凱 5 層 / HC 三層比較
- [[Ingest-工作流]] — Ingest 的詳細流程
- [[RAG-vs-LLM-Wiki]] — 與 RAG 的核心差異
- [[自製-Claude-Code-Skills]] — `note-maintain` 把維護流水線一鍵化

## 來源

- [[個人學習/obsidian相關筆記/搭建屬於自己的 LLM Wiki - Karpathy 模式實作指南]]（學習筆記，Karpathy gist + 本 Vault 實作經驗）
