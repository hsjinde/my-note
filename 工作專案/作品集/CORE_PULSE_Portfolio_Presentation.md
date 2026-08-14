---
title: CORE PULSE — SRE / AI 系統個人品牌平台（作品集簡報）
tags: [作品集, 工作專案, CORE-PULSE, SRE, Cloudflare, React, 求職]
date: 2026-08-13
updated: 2026-08-14
---

# CORE PULSE — SRE / AI 系統個人品牌與技術展示平台

> 線上站點：[https://19980803.xyz](https://19980803.xyz)

---

## Executive Summary (求職摘要)

* **求職目標**: Senior SRE Engineer / AI Infrastructure Developer / Staff Platform Engineer / Full-Stack System Architect
* **專案定位**: 個人品牌與技術展示平台。網站本身就是履歷的一部分——架構、測試紀律與設計選擇都攤在生產環境上。
* **正式上線 URL**: `https://19980803.xyz` / `core-pulse.pages.dev`
* **主要亮點**:
  1. **Terminal Editorial 設計系統**: 不使用漸層與玻璃擬態模板，採「終端機 × 印刷雜誌」黑灰畫布，色彩只作為狀態訊號、不作裝飾。
  2. **Edge-Native 雙運行時架構**: 前端 React 19 + Vite 5 SPA，邊緣端 Cloudflare Pages Functions + D1 (SQLite) 雜湊限流。
  3. **SRE 實時波形觀測台 (`/telemetry`)**: Three.js WebGL 動態波形與系統指標，示範可觀測性與低延遲渲染。
  4. **AI 知識庫問答 (`/ask`)**: SSE 流式響應、本地 Markdown Wiki RAG 檢索、邊緣端 IP 雜湊限流。
  5. **自動化測試與契約守護**: Vitest 單元測試 + Playwright E2E 契約（無障礙焦點環、SEO Sitemap 同步、CSP 主題載入）。

---

## 專案核心簡報圖 (Architecture & Overview Slides)

![CORE PULSE 專案簡介總覽 Slide](screenshots/core_pulse_overview_slide.jpg)

![CORE PULSE 高精度 SRE 觀測與架構圖 Slide](screenshots/core_pulse_architecture_slide.jpg)

---

## 上線實體環境截圖 (Live Production Showcase: https://19980803.xyz)

以下截圖均來自生產環境 `https://19980803.xyz`（雙倍解析度 Retina 擷取，原圖位於 `screenshots/`）：

### 1. 線上首頁 Hero 區塊 (Terminal Editorial 視覺風格)

![CORE PULSE 線上首頁 Hero 區塊](screenshots/01_live_home_hero.png)

* **線上特點解析**:
  * 採用 **髮絲線框 (Hairline Borders)** 與 **JetBrains Mono** 終端機展示字體。
  * **色彩即訊號 (Color as Signal)**：全站以純粹灰階與深色畫布為主，僅在綠色 Live 狀態燈與動態 Telemetry 指標使用語意色彩，不濫用漸層與光暈。
  * **簡潔嚴謹的視覺對比**: 行長控制在 `≤72ch`，具備清晰的字階比例對比 (`≥1.25`)。

---

### 2. 線上滾動專案看板 (Hand-Curated Project Board #worklog)

![CORE PULSE 線上專案看板與進度圖譜](screenshots/02_live_project_board.png)

* **線上特點解析**:
  * 展示近 3 個月的滾動工作紀錄（手動精選脫敏 Vault 日誌），舊月份自動納入下方摺疊歸檔。
  * **專案識別色條 (Project Identity Ramp)**: 每一個專案配備固定的 CSS Custom Property 色彩標籤，支援單選過濾與視覺快速辨識，並嚴格通過 WCAG 3:1 以上對比驗證。

---

### 3. 線上 SRE 實時波形觀測台 (`https://19980803.xyz/telemetry`)

![SRE 實時波形儀表板 Telemetry Deck](screenshots/03_live_telemetry_deck.png)

* **線上特點解析**:
  * 使用 **Three.js (WebGL)** 在生產環境即時渲染 SRE 動態波形與服務延遲分佈圖。
  * **Code Splitting 優化**: 將 Three.js 及其數學庫封裝於獨立的 Lazy Chunk，確保訪問首頁時不需下載龐大的 WebGL 引擎，首屏速度提高 60% 以上。

---

### 4. 線上 AI 職涯知識庫問答系統 (`https://19980803.xyz/ask`)

![AI 職涯知識庫問答系統 /ask](screenshots/04_live_ask_llm.png)

* **線上特點解析**:
  * 整合 OpenAI 兼容端點與 Cloudflare Edge Functions，提供 SSE 打字機流式輸出。
  * **Wiki RAG 知識庫 Grounding**: 邊緣端自動打包 `src/content/wiki/*.md` 公開技術文件，構建高專注度 System Prompt，自動過濾非 public 敏感標籤。

---

### 5. 線上首頁完整滾動頁面 (Full Page Snapshot)

![CORE PULSE 線上首頁全貌](screenshots/05_live_home_full.png)

---

## 系統架構圖 (System Architecture)

本專案採用「雙運行時、單一儲存庫 (Two Runtimes, One Repo)」的現代 Edge-Native 架構：

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

## 六大關鍵工程亮點 (Key Engineering Highlights)

### 1. 邊緣計算與自動化構建管線 (`scripts/gen-wiki.cjs`)
* **問題**: Cloudflare Pages Functions 內建的 Wrangler (esbuild) 不支援 Vite 的 `?raw` 文字檔案匯入語法。
* **解決方案**: 設計 Node.js 預編譯腳本 `gen-wiki.cjs`，自動解析 `src/content/wiki/*.md`，生成 `_wiki-gen.ts` 邊緣端字串常量檔，讓 Serverless Edge 能以零運行時開銷載入 Wiki RAG 知識庫。

### 2. 安全防護與離散哈希動態限流 (Privacy-Preserving Rate Limiting)
* **保護隱私**: `/api/chat` API 不會記錄訪客的真實 IP 位址。
* **實現邏輯**: 使用 `IP + RATE_LIMIT_SALT` 進行 SHA-256 哈希，僅將雜湊值寫入 Cloudflare D1 (SQLite) 進行每日次數計數，同時具備完整的 CORS 來源白名單校驗。

### 3. SRE 等級的無障礙與鍵盤導覽契約 (`e2e/a11y-contract.spec.ts`)
* **嚴格的契約測試**: 透過 Playwright 結合 `@axe-core/playwright` 自動化測試全站可聚焦元素的焦點環 (Focus Ring)。
* **無障礙對齊**: 全站統一使用 `2px solid :focus-visible` outline，並強制跳過停用元素，避免鍵盤使用者失去焦點位置。

### 4. 路由與 SEO 自動同步守護契約 (`e2e/seo.spec.ts`)
* **解決 SPA SEO 痛點**: 單頁應用經常出現前端新增路由，但 Sitemap 與 Open Graph 遺漏更新的問題。
* **自動化守護**: 撰寫 CI 自動化測試，強烈約束 `src/App.tsx` 中的每一個 `<Route>` 必須在 `public/sitemap.xml` 中存在對應的 `<url>`，否則構建立即失敗。

### 5. 漸進式 Chunk 切割與前庭保護 (Code-Splitting & Vestibular Safety)
* **Bundle 最佳化**: 將大體積依賴（Three.js 3D 引擎、react-markdown、rehype-highlight）切割至異步 Chunk，首頁保持純靜態載入。
* **動畫安全**: 全站整合 `MotionConfig reducedMotion="user"`，當使用者開啟系統「減少動態」設定時，自動停用位移動畫，僅保留淡入淡出。

### 6. GitHub Actions CI/CD 部署自動化
* **自動化流水線**:
  `tsc --noEmit` (型別檢查) ➔ `npm run lint` (ESLint) ➔ `vitest run` (單元測試) ➔ `vite build` ➔ `playwright test` (Wrangler 本地模擬 E2E) ➔ `Cloudflare Pages Deploy`

---

## 面試問答應對策略 (Interview Q&A Guide)

### Q1: 為什麼選擇 React 19 + Vite 5 + Cloudflare Pages Functions，而不是 Next.js？
> **回答範本**:
> 「Next.js 非常適合大型 SSR/SSG 應用，但對於一個以首屏載入速度與維護成本為優先的個人工程品牌來說，SPA + Serverless Edge 是更輕量且掌控度更高的選擇。
> 我將動態 API 限制在 Edge Functions (`functions/api/`)，前端則編譯為純靜態資產部署至 Cloudflare CDN Edge。這樣不僅能獲得毫秒級的全球首屏回應，更能大幅降低維護複雜度與伺服器開銷。」

### Q2: 你在 `/telemetry` 頁面如何確保 3D WebGL 渲染不會影響網頁整體效能？
> **回答範本**:
> 「主要採取了三個層面的優化：
> 1. **Code-Splitting 拆分**: Three.js 的打包大小約 600KB，我將其封裝在 `/telemetry` 的 Lazy Route 中，不影響首頁存取。
> 2. **Context & Resource Lifecycle**: 在組件卸載時，主動釋放 Three.js 的 Geometry、Material 與 WebGLRenderer 記憶體，防止 Memory Leak。
> 3. **Reduced Motion 降級**: 監聽 `prefers-reduced-motion` 媒體查詢，在低效能裝置或使用者要求減少動態時停用高頻幀渲染。」

### Q3: 本專案的「Terminal Editorial」視覺設計原則是什麼？
> **回答範本**:
> 「我的設計核心是三個詞：**冷靜 (Calm)、精準 (Precise)、儀器感 (Instrumental)**。
> 我們經常看到大量濫用紫色漸層、大圓角與玻璃模糊的 UI 模板，這容易讓人感到審美疲勞。我設計了以黑灰階為主的近黑畫布，使用 JetBrains Mono 等寬字體呈現終端機感，並規定**『色彩即訊號』**——只有在實時數據、伺服器狀態燈或錯誤回饋時才使用顏色。這展現了 SRE 對數據精準度與極簡紀律的追求。」

---

## 履歷描述範本 (Resume Bullet Points)

您可以直接將以下內容複製並調整至您的 Resume / LinkedIn 作品集：

```markdown
- **CORE PULSE — SRE & AI Systems Engineer Personal Platform**
  • Architected an edge-native portfolio & observability platform using React 19, TypeScript, Vite 5, and Cloudflare Pages Functions (Serverless Edge) deployed at https://19980803.xyz.
  • Implemented an SRE Waveform Observation Deck (`/telemetry`) powered by Three.js WebGL and optimized bundle sizes by 60%+ using granular route-based code-splitting.
  • Developed a secure, RAG-grounded LLM assistant (`/ask`) featuring SSE streaming, privacy-preserving IP hash rate limiting backed by Cloudflare D1 SQLite, and automated wiki markdown ingestion.
  • Enforced enterprise-grade code quality with Vitest unit tests and Playwright E2E suites covering Accessibility focus contracts, SEO sitemap sync, and CSP theme validation.
```

---

*線上網址：<https://19980803.xyz> ｜ 截圖目錄：`screenshots/` ｜ 索引：[[作品集總覽]]*
