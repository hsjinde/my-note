---
title: 大阪旅券 OSAKA TRIP — 和風手帳設計的全棧旅遊儀表板（作品集簡報）
tags: [作品集, 工作專案, Osaka-web, React, Cloudflare, Jamstack, 設計系統, 求職]
date: 2026-08-13
updated: 2026-08-14
---

# 大阪旅券 OSAKA TRIP — 離線可用、和風手帳設計的全棧旅遊儀表板

> 以 Obsidian Markdown 為資料來源的 Jamstack 應用：CI 建置成靜態站，收藏與待辦狀態存在 Cloudflare D1，斷線時退回 localStorage、連線後補送。

---

## Executive Summary (求職摘要與專案定位)

* **求職目標**: Senior Frontend Engineer / Full-Stack Web Architect / Jamstack Systems Specialist / UI/UX Systems Developer
* **專案名稱**: 大阪旅券 OSAKA TRIP (`Osaka-web`)
* **線上真實環境**: [https://osaka.19980803.xyz](https://osaka.19980803.xyz) （Cloudflare Pages 自訂網域）
* **備援網址**: [https://hsjinde.github.io/Osaka-web/](https://hsjinde.github.io/Osaka-web/) （GitHub Pages 雙部署）
* **專案儲存庫**: [hsjinde/Osaka-web](https://github.com/hsjinde/Osaka-web) (前端與 Worker) | [hsjinde/Osaka-vault](https://github.com/hsjinde/Osaka-vault) (Obsidian 知識庫源頭)
* **核心技術棧**: React 18, TypeScript, Vite, Tailwind CSS, Cloudflare Worker (Hono), Cloudflare D1 (Edge SQLite), Zod Validation, GitHub Actions CI/CD Pipeline, Vitest, Playwright.

---

## Real Production Screenshots (真實線上環境展示)

### 1. 桌機端首屏視覺 (Desktop Hero & Washi Editorial Interface)
![Osaka Web 桌機端首屏畫面](screenshots/osaka_web_live_desktop_hero.png)

> **設計觀點**:
> 捨棄 SaaS/Notion 那種中性樣板風格，以「御朱印帳手帳」為品牌意象。和紙紋理底色（`#F1EBDD`）、襯線標題（Shippori Mincho）與朱印紅（`#B23A1E`）組成整套配色。

---

### 2. 行動端單手操作體驗 (Mobile-First Handheld UX)
![Osaka Web 行動端介面展示](screenshots/osaka_web_live_mobile_hero.png)

> **旅途優先 (Travel-First)**:
> 針對旅途中站在大阪街頭單手查行程的情境，提供膠囊形導航列（Chips）、大字級對比、微動畫回饋與 ≥36–44px 觸控點擊區域。

---

### 3. 全頁面內容結構 (Desktop Full Page View)
![Osaka Web 桌機全頁面展示](screenshots/osaka_web_live_desktop_full.png)

---

### 4. 行動端完整頁面流轉 (Mobile Full Experience)
![Osaka Web 行動端全頁面展示](screenshots/osaka_web_live_mobile_full.png)

---

## 專案緣起與產品痛點 (Problem Statement & Product Purpose)

在多人家族旅行時，傳統旅遊規劃常面臨以下顯著痛點：
1. **資訊極度分散**: 餐廳攻略、景點網址、飯店確認單散落在 LINE 群組訊息與私人筆記中，旅途中翻找極為耗時。
2. **網路連線不穩定與載入延遲**: 到了海外旅遊現場，行動網路常有延遲，需要一個手感極速、甚至支援離線緩存的介面。
3. **多人協同與跨裝置狀態同步問題**: 大家想去的地方不同、行程進度與待辦勾選狀況需要跨手機/桌機即時同步，但又不想讓隨意瀏覽的訪客污染主選清單。

### 解決方案
以 Obsidian Markdown 作為單一資料來源（Single Source of Truth），經 CI/CD 建置管線轉為靜態 Web App；搭配 Cloudflare Edge Worker 與 D1，做到「不需刷新、離線可用、讀寫權限分離」的跨裝置同步儀表板。

---

## 系統架構與資料管線 (System Architecture & Pipeline)

本專案採 Jamstack 結合邊緣運算 (Edge Computing) 的解耦架構：

```mermaid
flowchart TD
    subgraph Source["Knowledge Layer (Obsidian)"]
        Vault["Osaka Vault (Markdown Files)"]
        EntityMD["wiki/entities/ (美食/景點/購物)"]
        ItineraryMD["wiki/dashboard/每日行程.md"]
        TodoMD["Osaka Trip/待辦.md"]
    end

    subgraph Pipeline["Build & Validation Pipeline"]
        GHActions["GitHub Actions (notify-dashboard)"]
        BuildScript["scripts/build-data.ts (Node.js)"]
        ZodValidator["Zod Schema Validation & Regex Parser"]
        JSONOutput["src/data/*.json (Type-Safe Data)"]
    end

    subgraph Hosting["Edge Distribution"]
        ViteBuild["Vite Static Production Bundle"]
        CFPages["Cloudflare Pages (Custom Domain)"]
        GHPages["GitHub Pages (Sync Backup)"]
    end

    subgraph EdgeState["Edge State Engine"]
        UserBrowser["User Mobile/Desktop Browser"]
        WorkerAPI["Cloudflare Worker (Hono Framework)"]
        EdgeD1["Cloudflare D1 (Edge SQLite Database)"]
        GHWriteback["GitHub REST API (Itinerary Commit Writeback)"]
    end

    Vault --> GHActions
    GHActions --> BuildScript
    BuildScript --> ZodValidator
    ZodValidator --> JSONOutput
    JSONOutput --> ViteBuild
    ViteBuild --> CFPages & GHPages

    UserBrowser <-->|1. Read Static Content (0ms)| CFPages
    UserBrowser <-->|2. Fetch/Put Favorites & Todos| WorkerAPI
    WorkerAPI <-->|3. Edge SQL Queries| EdgeD1
    WorkerAPI -.->|4. Direct Markdown Edit Writeback| GHWriteback
```

---

## 關鍵工程亮點與技術挑戰 (Engineering Highlights)

### 1. 和風手帳設計系統 (Washi Editorial Design System)
* **Creative North Star —「大阪旅券・御朱印帳」**:
  * **字體雙軌架構**: 標題與大數字使用經典襯線體 `Shippori Mincho` 營造手帳儀式感；正文與 UI 標籤採用 `Noto Sans TC` 確保閱讀可讀性。
  * **Strict Constraint — The One Seal Rule**: 朱印紅（`#B23A1E`）作為唯一的主聲音色，畫面上填色面積嚴格控制在 **≤10%**（僅用於主行動、當前分頁、極重要警示標記）。
  * **The No-Grey Rule**: 不用低飽和的中性灰，改用墨色 (`#29231A`) 與暖茶梯度，輔以 1.8% 橫向紙紋。
  * **狀態互動**: 蓋章進場 (`stampIn`)、愛心彈跳 (`heartPop`) 與同步呼吸動態 (`sealPulse`)，皆支援 `prefers-reduced-motion` 降級。

### 2. 雙重模式離線韌性狀態機 (Offline-Resilient Dual-Mode State Engine)
* **公開唯讀與授權寫入 (Read-Only & Auth Split)**:
  * 訪客免輸入密碼即可即時讀取遠端 Cloudflare D1 中的最新同步狀態。
  * 設定通行密碼後，瀏覽器持有 Bearer Token，發起 HTTP `PUT` 請求更新 D1。
* **離線與網路故障容錯 (Offline Fallback)**:
  * 若旅途中連線中斷或 Worker 暫時無法連通，狀態將自動儲存於 `localStorage`（離線模式），連線恢復後自動補送。

### 3. Zod 牆與 CI/CD 格式守門者 (Type-Safety & Build Integrity)
* **嚴格的型別保護門檻**:
  * 編輯 Obsidian 時，建置腳本 `scripts/build-data.ts` 會針對所有 Markdown 進行 Frontmatter 與正文解析，若欄位格式不符合 Zod Schema（如日期非 `YYYY-MM-DD` 或缺少必填屬性），CI/CD 立即拋錯並回報行號，格式錯誤不會進到生產環境。

### 4. 兩階段 API 雙向回寫 (GitHub API Content Splice Writeback)
* 收藏與待辦記在 Edge D1；當在前端調整每日行程結構時，Cloudflare Worker 透過 GitHub REST API 以 Base64 與 SHA 防衝突機制置換 Markdown 區段（`spliceItinerary`）並 Commit 回儲存庫，自動觸發重新部署。

---

## 軟體測試與工程品質 (Testing & Quality Assurance)

本專案的自動化測試涵蓋範圍：

* **前端 Store & Parser 測試**: **54 個 Vitest 測試套件** 覆蓋資料剖析、狀態切換與狀態轉化。
* **Worker API 測試**: **4 個 Vitest Worker API 測試** 驗證 Hono 路由、Bearer 權限與 D1 SQL 操作。
* **無障礙規範 (Accessibility)**: 達 WCAG AA 對比度標準（正文對比 ≥4.5:1），觸控目標大小控制在 ≥36–44px。

---

## 面試時可以談的四件事 (Talking Points)

1. **從使用情境反推架構 (Product-Minded Engineering)**:
   設計是從情境長出來的——關西街頭、單手持手機、弱網。可以談為什麼選 SSG 而不是 SSR，以及觸控區大小與導航形式的取捨。
2. **Jamstack 與 Edge 的分工**:
   靜態內容走 CDN、狀態走 Worker + D1、離線走 localStorage。可以談三者的邊界劃在哪、以及補送機制怎麼避免覆蓋。
3. **設計系統與規範文件**:
   獨立撰寫 `DESIGN.md` 與 `PRODUCT.md`，從色階、字體到 WCAG 對比都有明文規範。可以談「朱印紅 ≤10% 面積」這類硬性約束為什麼有用。
4. **CI/CD 與資料格式守門**:
   Git Commit 觸發建置、Zod 檢核 Frontmatter、Vitest 覆蓋 store 與 Worker API——資料源是人手寫的 Markdown，這層檢核是必要的而非裝飾。

---
*索引：[[作品集總覽]]*
