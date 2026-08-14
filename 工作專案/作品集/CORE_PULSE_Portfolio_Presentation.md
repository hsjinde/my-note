---
title: CORE PULSE — SRE / AI 系統個人品牌平台（作品集簡報）
tags: [作品集, 工作專案, CORE-PULSE, SRE, Cloudflare, React, 求職]
date: 2026-08-13
updated: 2026-08-14
---

# CORE PULSE — SRE / AI 系統個人品牌與技術展示平台

> 線上站點：[19980803.xyz](https://19980803.xyz)（備援 `core-pulse.pages.dev`）

---

## 求職摘要

* **求職目標**：Senior SRE Engineer / AI Infrastructure Developer / Staff Platform Engineer / Full-Stack System Architect
* **專案定位**：個人品牌與技術展示平台。網站本身就是履歷的一部分——架構、測試紀律與設計選擇都攤在生產環境上。
* **主要亮點**
  1. **Terminal Editorial 設計系統**：不用漸層與玻璃擬態模板，採「終端機 × 印刷雜誌」黑灰畫布，色彩只作為狀態訊號。
  2. **雙運行時架構**：前端 React 19 + Vite 5 SPA，邊緣端 Cloudflare Pages Functions + D1 雜湊限流。
  3. **`/telemetry` 實時波形觀測台**：Three.js WebGL 渲染服務指標與延遲分佈。
  4. **`/ask` AI 知識庫問答**：SSE 流式輸出、本地 Markdown Wiki RAG、邊緣端 IP 雜湊限流。
  5. **測試即契約**：Vitest 單元測試 + Playwright E2E（無障礙焦點環、SEO sitemap 同步、CSP 主題載入）。

---

## 專案簡報圖

![CORE PULSE 專案簡介總覽 Slide](screenshots/core_pulse_overview_slide.jpg)

![CORE PULSE 高精度 SRE 觀測與架構圖 Slide](screenshots/core_pulse_architecture_slide.jpg)

---

## 線上畫面

以下皆為生產環境 `https://19980803.xyz` 的實際畫面（Retina 雙倍解析度擷取）。

### 1. 首頁 Hero

![CORE PULSE 線上首頁 Hero 區塊](screenshots/01_live_home_hero.png)

髮絲線框搭配 JetBrains Mono。全站灰階深色畫布，只有 Live 狀態燈與 Telemetry 指標用得上顏色——設計規則是「色彩即訊號」，不做裝飾用途。行長 `≤72ch`，字階比例 `≥1.25`。

### 2. 滾動專案看板

![CORE PULSE 線上專案看板與進度圖譜](screenshots/02_live_project_board.png)

近三個月的工作紀錄（從 Vault 日誌手動精選並脫敏），更舊的月份自動收進下方摺疊區。每個專案有固定的 CSS Custom Property 色標，可單選過濾，對比度皆通過 WCAG 3:1。

### 3. `/telemetry` 實時波形觀測台

![SRE 實時波形儀表板 Telemetry Deck](screenshots/03_live_telemetry_deck.png)

Three.js 在生產環境即時渲染波形與服務延遲分佈。整包 WebGL 引擎切在這條 lazy route 裡，進首頁不會下載到。

### 4. `/ask` AI 職涯知識庫

![AI 職涯知識庫問答系統 /ask](screenshots/04_live_ask_llm.png)

Edge Function 串 OpenAI 相容端點，SSE 打字機輸出。知識來源是站內 `src/content/wiki/*.md`，建置時打包進邊緣端，非 public 標籤會被過濾掉。

### 5. 首頁全頁

![CORE PULSE 線上首頁全貌](screenshots/05_live_home_full.png)

---

## 系統架構

「雙運行時、單一儲存庫」：前端編譯成靜態資產走 CDN，動態部分限縮在 Pages Functions。

```mermaid
flowchart TD
    subgraph Client ["Client Runtime (Browser / React 19 SPA)"]
        UI["React 19 + Vite 5 App"]
        Router["React Router v7"]
        Home["Home Page (Static Bundle)"]
        Telemetry["Telemetry Page (Lazy Chunk + Three.js)"]
        AskUI["Ask LLM Page (Lazy Chunk + Markdown)"]
    end

    subgraph Edge ["Serverless Edge (Cloudflare Pages Functions)"]
        CF["functions/api/"]
        ChatEP["/api/chat (SSE Stream Endpoint)"]
        WikiGen["functions/api/_wiki-gen.ts (Auto-generated Wiki)"]
        Sanitizer["chat-sanitizer.ts (Input Protection)"]
        RateLimit["chat-rate-limit.ts (IP Hash Rate Limiter)"]
    end

    subgraph External ["External Infrastructure & Storage"]
        D1[("Cloudflare D1 (SQLite) Rate Limit Table")]
        LLM["OpenAI-Compatible LLM API"]
        WikiFiles["src/content/wiki/*.md"]
    end

    UI --> Router
    Router --> Home
    Router --> Telemetry
    Router --> AskUI

    AskUI -- "POST /api/chat (SSE)" --> ChatEP
    ChatEP --> Sanitizer
    ChatEP --> RateLimit
    RateLimit -- "Read/Write Hits" --> D1
    ChatEP --> WikiGen
    WikiGen <--- "Build Time (gen-wiki.cjs)" --- WikiFiles
    ChatEP -- "Stream Tokens" --> LLM
    LLM -- "SSE Delta Frames" --> AskUI
```

---

## 六個工程決策

### 1. 建置期預編譯 Wiki（`scripts/gen-wiki.cjs`）

Pages Functions 用的 Wrangler（esbuild）不支援 Vite 的 `?raw` 文字匯入，Edge 端讀不到 Markdown。解法是寫一支 Node 預編譯腳本，把 `src/content/wiki/*.md` 轉成 `_wiki-gen.ts` 字串常量，Edge 端載入知識庫的運行時開銷是零。

### 2. 不記 IP 的限流

`/api/chat` 不記錄訪客真實 IP。作法是 `IP + RATE_LIMIT_SALT` 取 SHA-256，只把雜湊值寫進 D1 做每日計數，另有 CORS 來源白名單。要防濫用，但不需要知道誰是誰。

### 3. 無障礙焦點環契約（`e2e/a11y-contract.spec.ts`）

Playwright 搭 `@axe-core/playwright` 掃過全站可聚焦元素，驗證焦點環存在。全站統一 `2px solid :focus-visible` outline，停用元素強制跳過，鍵盤使用者不會失去焦點位置。這是 CI 會擋的契約，不是文件上的承諾。

### 4. SEO sitemap 同步守護（`e2e/seo.spec.ts`）

SPA 常見的問題是前端加了新路由，但 sitemap 與 Open Graph 忘了更新。所以寫成測試：`src/App.tsx` 裡每一個 `<Route>` 都必須在 `public/sitemap.xml` 找到對應的 `<url>`，否則 build 直接失敗。

### 5. Chunk 切割與動畫安全

Three.js、react-markdown、rehype-highlight 這些大體積依賴全切進 async chunk，首頁維持純靜態載入，首屏速度改善 60% 以上。動畫方面全站掛 `MotionConfig reducedMotion="user"`，使用者開了系統「減少動態」就只留淡入淡出。

### 6. CI/CD

`tsc --noEmit` ➔ `npm run lint` ➔ `vitest run` ➔ `vite build` ➔ `playwright test`（Wrangler 本地模擬）➔ Cloudflare Pages 部署。

---

## 面試問答

### Q1：為什麼是 React 19 + Vite 5 + Pages Functions，而不是 Next.js？

> 「Next.js 適合大型 SSR/SSG 應用，但這個站的優先順序是首屏速度與維護成本，SPA + Serverless Edge 更輕、掌控度更高。
> 我把動態 API 限縮在 `functions/api/`，前端編譯成純靜態資產丟到 Cloudflare CDN。換來的是毫秒級全球首屏，以及低很多的維護複雜度。」

### Q2：`/telemetry` 的 WebGL 渲染怎麼不拖垮整站？

> 「三層處理：
> 1. **Code-splitting**：Three.js 打包約 600KB，封在 `/telemetry` 的 lazy route，不影響首頁。
> 2. **資源生命週期**：組件卸載時主動釋放 Geometry、Material 與 WebGLRenderer，避免 memory leak。
> 3. **降級**：監聽 `prefers-reduced-motion`，低效能裝置或使用者要求減少動態時停用高頻幀渲染。」

### Q3：「Terminal Editorial」的設計原則是什麼？

> 「三個詞：冷靜、精準、儀器感。
> 現在很多 UI 模板濫用紫色漸層、大圓角與玻璃模糊，看久了會疲乏。我用近黑灰階畫布配 JetBrains Mono 等寬字體做終端機感，並定下『色彩即訊號』——只有實時數據、狀態燈或錯誤回饋才用得上顏色。這其實就是 SRE 看儀表板的習慣。」

---

## 履歷描述範本

### 繁體中文

* **設計與建置 Edge-Native 個人品牌與可觀測性平台 (CORE PULSE)**：以 React 19、TypeScript、Vite 5 與 Cloudflare Pages Functions 打造雙運行時架構，前端編譯為靜態資產走 CDN，動態 API 限縮於 Serverless Edge，正式上線於 `https://19980803.xyz`。
* **開發 SRE 實時波形觀測台 (`/telemetry`)**：以 Three.js WebGL 即時渲染服務指標與延遲分佈，透過 route-based code-splitting 將大體積依賴切至 async chunk，首屏載入速度改善 60% 以上。
* **建置具隱私保護之 RAG 問答系統 (`/ask`)**：整合 SSE 流式輸出與建置期預編譯的 Markdown Wiki 知識庫，並以 `IP + Salt` SHA-256 雜湊寫入 Cloudflare D1 進行限流——達成防濫用的同時不記錄訪客真實 IP。
* **將工程規範寫成 CI 契約**：以 Vitest 單元測試搭配 Playwright E2E 守護無障礙焦點環、SEO sitemap 與路由同步、CSP 主題載入；任一路由未同步至 `sitemap.xml` 即中斷建置。

### English

* **Architected an Edge-Native Personal Brand & Observability Platform (CORE PULSE)**: Built a dual-runtime system with React 19, TypeScript, Vite 5 and Cloudflare Pages Functions—static assets served from CDN, dynamic APIs confined to the serverless edge—deployed at https://19980803.xyz.
* **Implemented an SRE Waveform Observation Deck (`/telemetry`)**: Rendered live service metrics and latency distributions with Three.js WebGL, improving first-paint performance by 60%+ through granular route-based code-splitting of heavy dependencies.
* **Developed a Privacy-Preserving RAG-Grounded LLM Assistant (`/ask`)**: Combined SSE streaming with a build-time precompiled markdown wiki knowledge base, and rate-limited requests via salted SHA-256 IP hashes stored in Cloudflare D1—preventing abuse without ever recording visitor IPs.
* **Encoded Engineering Standards as CI Contracts**: Enforced accessibility focus-ring behaviour, SEO sitemap/route parity and CSP theme loading through Vitest unit tests and Playwright E2E suites; any route missing from `sitemap.xml` fails the build outright.

---
*線上網址：<https://19980803.xyz> ｜ 索引：[[作品集總覽]]*
