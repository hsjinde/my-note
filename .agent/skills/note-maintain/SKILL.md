---
name: note-maintain
description: "Use when the user asks to maintain, clean up, or lint their Obsidian/Markdown note vault — 筆記維護, 整理筆記, lint, lint wiki, 健康檢查, 消化 Clippings, 消化剪藏, 更新 core rules, 同步 CLAUDE/AGENTS — or says a bare 'lint' inside the note vault project. Runs the full maintenance pass in one command: wiki health check → auto-fix safe issues → ingest pending Clippings → verify rule-file sync → log."
---

# Note Maintain（vault 一鍵維護）

## Overview

常見的筆記維護流程要使用者手動跑好幾輪：「lint」→「全部修」→「消化 Clippings」→「提交」。這個 skill 把它變成一個指令：**能自動修的直接修，需要決策的整理成一份清單一次問完**。

以使用者的語言回報。

## 設定（安裝後先依你的 vault 調整）

本 skill 假設 vault 具備以下結構，路徑不同請直接改本檔案：

| 項目 | 預設假設 | 說明 |
|------|----------|------|
| vault 路徑 | 目前專案根目錄 | 你的 Obsidian/Markdown 筆記庫位置 |
| 規則檔 | `core_rules.md` | 唯一真相來源；`CLAUDE.md`／`AGENTS.md` 只是指向它的指標檔 |
| 知識庫目錄 | `wiki/` | 提煉後的筆記與 `wiki/index.md` 總索引 |
| 剪藏目錄 | `Clippings/` | 待消化的網頁剪藏（唯讀，不修改原文） |
| 維護日誌 | `wiki/log.md` | 每次維護的紀錄 |

## When to Use

- 使用者說：lint、筆記維護、整理筆記、健康檢查、消化 Clippings
- 在筆記 vault 專案裡說單字「lint」
- 距上次維護超過一個月，或 ingest 已累積約 10 次（見維護日誌）

**不適用**：單篇筆記的查詢或編輯（直接處理即可）、結構不同的其他 vault（原則可借用，路徑規則需另行調整）。

## Workflow

**執行前必讀**：規則檔 + `wiki/index.md`。若 vault 專案內已有對應的細部 skills（例如 lint／ingest 工作流），優先委派給它們，本 skill 只負責串流程，不重造輪子。

1. **健康檢查**：
   - 孤立頁：wiki 內沒有任何入鏈的頁面
   - 斷鏈：指向不存在頁面的 `[[Wikilink]]`
   - 矛盾與過時：同一概念在不同頁面說法衝突、或內容已被新筆記推翻
   - 未消化的 Clippings：比對 `Clippings/` 與維護日誌，列出還沒 ingest 的檔案
2. **分流**：
   - **直接修（不用問）**：斷鏈修復、格式修正、`wiki/index.md` 補漏、孤立頁補上合理入鏈
   - **整理成清單一次問（編號選項）**：內容矛盾要留哪邊、過時頁面刪或改寫、要不要 ingest 某篇 Clipping。選項要編號，方便使用者用「全部修」「直接修掉 1–4」回覆。
3. **消化 Clippings**：對使用者選定（或全部）的未消化剪藏，提煉重點寫進 `wiki/` 並更新索引與入鏈。**原始檔唯讀**——提煉進 wiki，不改 Clippings 原文。
4. **規則檔同步檢查**：確認 `CLAUDE.md`、`AGENTS.md` 仍只是指向規則檔的指標檔、沒有長出重複內容；若規則檔本次有改動，檢查指標檔的補充段落是否與新規則矛盾。
5. **記錄**：把本次維護做了什麼寫進維護日誌（日期、修了幾個斷鏈、ingest 了哪些）。
6. **收尾**：回報變更摘要（幾頁修改、幾篇 ingest、剩餘待決事項）。**不主動 commit**——等使用者明確要求提交再走 git 流程。

## Quick Reference

| 使用者說 | 動作 |
|----------|------|
| 「lint」 | 步驟 1–2：檢查 + 自動修 + 出決策清單 |
| 「全部修」「直接修掉 1–4」 | 執行清單上指定項，修完回報 |
| 「消化 Clippings」「消化 @某檔案」 | 步驟 3，指定檔案就只處理那篇 |
| 「更新 core rules」 | 改規則檔後跑步驟 4 同步檢查 |
| 「提交」「ship it」 | 走 git 提交流程（或轉交專案的部署 skill） |

## Common Mistakes

| 錯誤 | 後果 |
|------|------|
| 把問題一個一個問 | 使用者要回覆 N 輪；應整理成一份編號清單一次問完 |
| 直接大改 Clippings／原始區檔案 | 違反唯讀原則；提煉進 wiki 才是正解 |
| 修完自動 commit | 越過使用者的 git 紀律 |
| 在 CLAUDE.md／AGENTS.md 裡新增規則內容 | 破壞單一真相來源，規則開始分岔 |
| 跳過維護日誌記錄 | 下次無法判斷「該不該 lint 了」 |

## Red Flags — 回報「維護完成」之前自問

- 維護日誌記錄了這次的動作嗎？
- 自動修的部分有沒有動到 Clippings 原文？有 → 還原，改成提煉進 wiki。
- 決策清單是一次給完，還是打算分批問？分批 → 合併成一份。
