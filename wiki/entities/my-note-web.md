---
title: my-note-web
tags: [專案, Cloudflare, Workers, RAG, Obsidian, Edge]
updated: 2026-08-17
source_count: 1
---

# my-note-web — Obsidian Vault 的 Edge 閱讀平台

[[Ethan]] 把本 vault 接上公開網頁的專案（`note.19980803.xyz`）：Cloudflare Workers + Hono 提供閱讀與檢索，GitHub Webhook 做增量同步，網頁編輯經 Contents API 樂觀鎖回寫。**這是 [[Quartz-閱讀網站]] 那份規劃的實際落地版本**——最後沒用 Quartz + GitHub Pages，改走 Workers 自建。

## 技術棧

| 層 | 技術 |
|---|---|
| 邊緣 | Cloudflare Workers、Hono、KV（分片）、Workers AI（`@cf/meta/llama-3-8b-instruct`） |
| 前端 | React 18 + Vite + TypeScript、Vanilla CSS（自建「書房紙頁」設計系統）、30 行自寫 Hash Router、markdown-it + 自訂 Wikilink plugin |
| 資料管線 | GitHub Webhook 增量同步、Contents API 樂觀鎖回寫、Gzip Tarball 串流全量同步 |
| 品質 | Vitest 15 檔 83 測試、TypeScript strict |

## 四個關鍵設計決策

### 1. Sharded KV：把全站導覽壓成一次讀取

KV 按讀寫次數計費，一篇一 key 會讓「畫目錄樹」變成一次 `list()` 加數百次 `get()`。改成兩層：`shard:<頂層資料夾>` 把同一頂層目錄的所有筆記併成單一 JSON（`Record<path, {content, sha}>`），`meta:index` 只放標題／標籤／摘要／wikilink 引用關係。效果是首頁一次 KV 讀取拿到全站 168+ 篇導航結構，邊緣 API `<50ms`。

### 2. 雙管線同步：Webhook 增量 + Tarball 災難復原

增量走 `X-Hub-Signature-256` HMAC 驗簽後只抓 push payload 的 `added`/`modified`/`removed`，push 到上線 3 秒內；全量（首次部署或索引遺失）不打上千次 File API，而是抓 repo gzip tarball 在 Worker 記憶體串流解壓後批次寫入。

### 3. 零 Embedding 的 RAG

語料只有數百篇，養 Vector DB 不划算。改用全記憶體兩階段計分：中文 Bigram、英數 token 切分 → Stage 1 掃 `meta:index`（標題 ×3、標籤 ×2）取 Top 10 → Stage 2 載入 shard 全文計頻率與鄰近度取 Top 4 → 連同來源路徑注入 system prompt。**失效條件寫明了**：語料再大一個數量級，關鍵字計分就贏不了語意檢索，那時才該換 Vector DB。同一個「先判斷規模再決定要不要上向量庫」的立場見 [[RAG-vs-LLM-Wiki]]。

### 4. 公開／私有硬邊界

`PUBLIC_FOLDERS` 白名單（`個人學習/`、`好工具推薦/`、`工作專案/`）之外的頂層目錄不進 `publicIndex()`；`wiki/` 照樣同步進 KV 但標記 `NoteMeta.private = true` 被硬性過濾，只有通過 HMAC session 驗證的登入者呼叫 `/api/ask` 時 RAG 才能檢索到它。白名單而非黑名單的理由與 [[Quartz-閱讀網站]] 當初的決策一致。

## 相關

- [[Ethan]] — 作者
- [[Quartz-閱讀網站]] — 本專案的前身規劃（Quartz + GitHub Pages，已被此專案取代）
- [[Obsidian]] — 內容來源平台
- [[LLM-Wiki]]、[[RAG-vs-LLM-Wiki]] — 站內問答的知識層與檢索取捨
- [[wiki/entities/CORE-PULSE|CORE-PULSE]]、[[Osaka-Web]]、[[Osaka-Vault]] — [[Ethan]] 的其他 Cloudflare 專案

## 來源

- [[工作專案/作品集/My_Note_Web_Portfolio_Presentation|my-note-web 作品集簡報]]（2026-08-14，架構、六個工程決策與測試）
