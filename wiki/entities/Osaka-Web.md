---
title: 大阪旅券 OSAKA TRIP（Osaka-web）
tags: [專案, React, Cloudflare, Jamstack, 設計系統, 離線]
updated: 2026-08-17
source_count: 1
---

# 大阪旅券 OSAKA TRIP — 離線可用的和風旅遊儀表板

[[Ethan]] 的 Jamstack 旅遊儀表板（`osaka.19980803.xyz`，另有 GitHub Pages 備援雙部署）。以 [[Osaka-Vault]] 的 Obsidian Markdown 為單一資料來源，CI 建置成靜態站；收藏與待辦狀態走 Cloudflare D1，斷線退回 `localStorage`、連線後補送。

## 為什麼做

多人家族旅行的三個實際痛點：資訊散落在 LINE 群組與各自筆記、海外弱網環境要載入快甚至離線可看、跨裝置多人協同但不能讓隨手瀏覽的訪客改到主清單。

## 技術棧

React 18 + TypeScript + Vite + Tailwind CSS ｜ Cloudflare Worker（Hono）+ D1 ｜ Zod ｜ GitHub Actions ｜ Vitest + Playwright

## 四個工程決策

### 1. 「御朱印帳」設計系統

北極星意象落成四條可執行規則：字體雙軌（標題襯線 `Shippori Mincho`、正文 `Noto Sans TC`）；**One Seal Rule**——朱印紅 `#B23A1E` 是唯一主色，填色面積硬性 ≤10%，只給主行動、當前分頁與重要警示；**No-Grey Rule** 改用墨色 `#29231A` 與暖茶梯度配 1.8% 紙紋；狀態動畫（`stampIn`／`heartPop`／`sealPulse`）全部支援 `prefers-reduced-motion` 降級。

### 2. 離線韌性與讀寫分離

訪客免密碼即可讀 D1 的最新同步狀態，寫入需 Bearer Token。連線中斷時狀態先落 `localStorage`，恢復後自動補送。

### 3. Zod 當建置期守門員

資料源是人手寫的 Markdown，格式一定有人寫錯。`scripts/build-data.ts` 建置時解析 Frontmatter 與正文，不符 schema（日期非 `YYYY-MM-DD`、缺必填欄位）就讓 CI 拋錯並指出行號——格式錯誤進不到生產環境。

### 4. 行程結構的 GitHub API 回寫

前端調整每日行程結構時，Worker 透過 GitHub REST API 以 Base64 + SHA 防衝突機制置換 Markdown 區段（`spliceItinerary`）並 commit 回儲存庫，自動觸發重新部署。與 [[my-note-web]] 的 Contents API 樂觀鎖是同一套「網頁改動要安全寫回 Git」的思路。

## 品質

前端 store 與 parser 54 個 Vitest 測試、Worker API 4 個；正文對比 ≥4.5:1（WCAG AA），觸控目標 ≥36–44px。設計與產品規範獨立寫在 `DESIGN.md` / `PRODUCT.md`。

## 相關

- [[Ethan]] — 作者
- [[Osaka-Vault]] — 本站的資料來源知識庫
- [[my-note-web]] — 同樣把 Obsidian 當資料來源、狀態走 Cloudflare 的姊妹專案
- [[wiki/entities/CORE-PULSE|CORE-PULSE]] — [[Ethan]] 的另一套自建設計系統（Terminal Editorial）

## 來源

- [[工作專案/作品集/Osaka_Web_Portfolio_Presentation|大阪旅券 OSAKA TRIP 作品集簡報]]（2026-08-14）
