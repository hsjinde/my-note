---
title: LLM Wiki
tags: [知識管理, AI, 概念]
updated: 2026-06-04
source_count: 2
---

# LLM Wiki

由 [[Andrej-Karpathy]] 提出的知識管理模式：讓 AI 持續建立並維護一個結構化的 Markdown Wiki，而不是每次查詢時從原始文件重新檢索。知識只需編譯一次，之後持續累積更新。

## 核心概念

- **持久累積**：知識寫入 Wiki 後跨 session 保留，不像 RAG 每次重新推導
- **交叉引用已就位**：矛盾、連結、合成在 ingest 時就處理好，查詢時直接使用
- **純文字優先**：核心是 Markdown 檔案，任何模型、任何平台都能讀取
- **人管來源，AI 管維護**：人負責選擇資料來源和提問，AI 負責寫作、更新、交叉引用

## 三層架構

| 層 | 名稱 | 說明 |
|---|---|---|
| 第一層 | **Raw Source** | 原始資料（文章、書籍、剪藏），不可修改 |
| 第二層 | **Wiki 頁面** | AI 生成的摘要、概念頁、實體頁，持續更新 |
| 第三層 | **Schema** | 定義 Wiki 結構的規則文件（如 AGENTS.md） |

## 工作流

| 指令 | 動作 |
|---|---|
| **Ingest** | 讀取新來源 → 提煉知識 → 建立/更新 Wiki 頁面 |
| **Query** | 先讀 index → 查 Wiki → 回答 → 可歸檔問答 |
| **Lint** | 健康檢查：孤立頁、重複概念、缺連結、過時內容 |
| **Log** | 每次操作後 append 歷史紀錄 |
| **Index** | Wiki 目錄，AI 每次操作前先讀這裡 |

## 與 RAG 的差別

見 [[RAG-vs-LLM-Wiki]]

## 架構演進

見 [[知識庫架構設計]]（Karpathy 原版 → 范凱 5 層 → HC 三層）

## 這個 Vault 的實作

- Wiki 路徑：wiki/
- Schema 規則：.claude/skills/llm-wiki/SKILL.md
- 操作日誌：wiki/log.md
- 工作流細節：[[Ingest-工作流]]

## 來源

- [[3 層架構打造個人 AI 大腦：從 Raw Data 到持久知識庫 🛠️]]（HC AI說人話，2026-04）
- [[Karpathy 的 LLM Wiki 火了，我改造了一下，比原版好用十倍]]（范凱說AI，2026-04）