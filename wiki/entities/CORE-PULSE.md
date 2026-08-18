---
title: CORE PULSE
tags: [專案, SRE, AI, 前端, Cloudflare, LLM-Wiki]
updated: 2026-08-17
source_count: 2
---

# CORE PULSE — SRE / AI 系統個人品牌平台

[[Ethan]] 的個人品牌與技術展示網站（`19980803.xyz`，備援 `core-pulse.pages.dev`）。網站本身即作品：架構、測試紀律與設計選擇都攤在生產環境上。以站內一份 LLM Wiki 為知識來源提供 AI 問答，是 [[LLM-Wiki]] 概念的一個對外應用。

> **狀態演進**：早期版本是「右下角 SVG 企鵝浮動 widget」形式的吉祥物聊天窗；該浮動 widget 已於 2026-07-18 移除，現況是獨立的 `/ask` 全頁問答。網址也由 `core-pulse.19980803.xyz` 收斂為主網域 `19980803.xyz`。（見 [[2026-08 工作紀錄]]）

## 站點結構

- **首頁 Hero**：Terminal Editorial 灰階深色畫布，髮絲線框 + JetBrains Mono。
- **滾動專案看板**：近三個月工作紀錄（自 vault 日誌手動精選並脫敏），每專案有 CSS 色標、可過濾，對比通過 WCAG 3:1。
- **`/telemetry` 實時波形觀測台**：Three.js WebGL 即時渲染服務指標與延遲分佈；整包 WebGL 引擎封在 lazy route，進首頁不會下載到。
- **`/ask` AI 職涯知識庫**：Edge Function 串 OpenAI 相容端點、SSE 打字機輸出；知識來源 `src/content/wiki/*.md` 建置時打包進邊緣端，非 `public` 標籤會被過濾。

## 技術棧

| 層 | 技術 |
|---|---|
| 前端 | React 19 + Vite 5 + TypeScript、React Router v7 |
| 邊緣 | Cloudflare Pages Functions（`functions/api/`）+ D1（限流表） |
| LLM | OpenAI 相容端點、SSE 串流 |
| 測試 | Vitest 單元 + Playwright E2E（`@axe-core/playwright`） |

## 關鍵設計決策

- **雙運行時、單一儲存庫**：前端編譯成靜態資產走 CDN，動態 API 限縮在 Pages Functions。
- **建置期預編譯 Wiki**（`scripts/gen-wiki.cjs`）：Wrangler（esbuild）不支援 Vite 的 `?raw` 匯入，故把 `src/content/wiki/*.md` 轉成 `_wiki-gen.ts` 字串常量，Edge 端載入知識庫的運行時開銷為零。
- **不記 IP 的限流**：`/api/chat` 取 `IP + RATE_LIMIT_SALT` 的 SHA-256，只把雜湊寫進 D1 做每日計數 + CORS 白名單。防濫用但不記錄訪客真實 IP。
- **測試即契約**：無障礙焦點環（全站 `2px :focus-visible` outline，axe 掃描）、SEO sitemap 同步（`App.tsx` 每個 `<Route>` 都必須在 `sitemap.xml` 找到對應 `<url>`，否則 build fail）、CSP 主題載入。

## Terminal Editorial 設計系統

近黑灰階畫布 + JetBrains Mono 等寬字體，「色彩即訊號」——只有實時數據、狀態燈或錯誤回饋才用得上顏色（SRE 看儀表板的習慣）。刻意不用漸層與玻璃擬態模板。與 [[Ethan]] 另一套設計系統（[[Osaka-Web]] 的和風御朱印帳）形成對照。

## 相關

- [[Ethan]] — 網站主人
- [[LLM-Wiki]] — `/ask` 回答的知識來源模式
- [[設計系統實踐]] — Terminal Editorial 是其中一套
- [[my-note-web]]、[[Osaka-Web]]、[[Osaka-Vault]]、[[Postfix-Manager]] — [[Ethan]] 的其他專案

## 來源

- [[工作專案/CORE-PULSE-AI吉祥物對話系統]]（早期系統架構與部署）
- [[工作專案/作品集/CORE_PULSE_Portfolio_Presentation|CORE PULSE 作品集簡報]]（2026-08-14，現況：`/telemetry`、`/ask` 全頁、CI 契約）
