---
title: 大阪旅券 OSAKA TRIP — 和風手帳設計的全棧旅遊儀表板（作品集簡報）
tags: [作品集, 工作專案, Osaka-web, React, Cloudflare, Jamstack, 設計系統, 求職]
date: 2026-08-13
updated: 2026-08-14
---

# 大阪旅券 OSAKA TRIP — 離線可用、和風手帳設計的全棧旅遊儀表板

> 以 Obsidian Markdown 為資料來源的 Jamstack 應用：CI 建置成靜態站，收藏與待辦狀態存在 Cloudflare D1，斷線時退回 localStorage、連線後補送。

---

## 求職摘要

* **求職目標**：Senior Frontend Engineer / Full-Stack Web Architect / Jamstack Systems Specialist / UI/UX Systems Developer
* **線上環境**：[osaka.19980803.xyz](https://osaka.19980803.xyz)（Cloudflare Pages 自訂網域）｜備援 [GitHub Pages](https://hsjinde.github.io/Osaka-web/) 雙部署
* **儲存庫**：[hsjinde/Osaka-web](https://github.com/hsjinde/Osaka-web)（前端與 Worker）｜[hsjinde/Osaka-vault](https://github.com/hsjinde/Osaka-vault)（Obsidian 資料來源）
* **技術棧**：React 18、TypeScript、Vite、Tailwind CSS、Cloudflare Worker (Hono)、Cloudflare D1、Zod、GitHub Actions、Vitest、Playwright

---

## 線上畫面

### 1. 桌機端首屏

![Osaka Web 桌機端首屏畫面](screenshots/osaka_web_live_desktop_hero.png)

不做 SaaS/Notion 那種中性樣板，品牌意象是「御朱印帳」。和紙紋理底色 `#F1EBDD`、襯線標題 Shippori Mincho、朱印紅 `#B23A1E`。

### 2. 行動端

![Osaka Web 行動端介面展示](screenshots/osaka_web_live_mobile_hero.png)

設計情境很具體：站在大阪街頭、單手拿手機、網路時好時壞。所以是膠囊形導航列、大字級對比、微動畫回饋，觸控區 ≥36–44px。

### 3. 桌機全頁

![Osaka Web 桌機全頁面展示](screenshots/osaka_web_live_desktop_full.png)

### 4. 行動端全頁

![Osaka Web 行動端全頁面展示](screenshots/osaka_web_live_mobile_full.png)

---

## 為什麼做這個

多人家族旅行的三個實際痛點：

1. **資訊散落**：餐廳攻略、景點網址、飯店確認單分散在 LINE 群組與各自的筆記裡，旅途中翻找很慢。
2. **弱網環境**：人在海外，行動網路不穩，需要載入快、甚至離線也能看的介面。
3. **跨裝置與多人協同**：行程進度與待辦勾選要即時同步，但又不想讓隨手瀏覽的訪客改到主清單。

解法是把 Obsidian Markdown 當單一資料來源，CI 建置成靜態站；狀態交給 Cloudflare Worker + D1，做到不需刷新、離線可用、讀寫權限分離。

---

## 系統架構

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

## 四個工程決策

### 1. 和風手帳設計系統

北極星是「大阪旅券・御朱印帳」，落成四條可執行的規則：

* **字體雙軌**：標題與大數字用襯線 `Shippori Mincho` 撐儀式感，正文與 UI 標籤用 `Noto Sans TC` 顧可讀性。
* **One Seal Rule**：朱印紅 `#B23A1E` 是唯一主色，畫面填色面積硬性限制 **≤10%**，只給主行動、當前分頁與重要警示。
* **No-Grey Rule**：不用低飽和中性灰，改用墨色 `#29231A` 與暖茶梯度，配 1.8% 橫向紙紋。
* **狀態互動**：蓋章進場 `stampIn`、愛心彈跳 `heartPop`、同步呼吸 `sealPulse`，全部支援 `prefers-reduced-motion` 降級。

### 2. 離線韌性狀態機

讀寫分離：訪客免密碼就能讀到 D1 裡的最新同步狀態；設定通行密碼後瀏覽器持有 Bearer Token，才能 `PUT` 更新。

旅途中連線中斷或 Worker 連不上時，狀態先寫進 `localStorage`，恢復連線後自動補送——這是為了現場實際會發生的情況而做，不是為了 demo。

### 3. Zod 當建置期守門員

資料源是人手寫的 Markdown，格式一定會有人寫錯。`scripts/build-data.ts` 在建置時解析所有 Frontmatter 與正文，欄位不符 Zod schema（日期不是 `YYYY-MM-DD`、缺必填欄位）就讓 CI 拋錯並指出行號。格式錯誤進不到生產環境。

### 4. 行程結構的 GitHub API 回寫

收藏與待辦記在 D1；但在前端調整每日行程結構時，Worker 會透過 GitHub REST API 以 Base64 加 SHA 防衝突機制置換 Markdown 區段（`spliceItinerary`）並 commit 回儲存庫，自動觸發重新部署。

---

## 測試與品質

* **前端 store 與 parser**：54 個 Vitest 測試，覆蓋資料剖析與狀態轉換。
* **Worker API**：4 個 Vitest 測試，驗證 Hono 路由、Bearer 權限與 D1 SQL 操作。
* **無障礙**：正文對比 ≥4.5:1（WCAG AA），觸控目標 ≥36–44px。

---

## 面試時可以談的四件事

1. **從使用情境反推架構**
   設計是從情境長出來的——關西街頭、單手持手機、弱網。可以談為什麼選 SSG 而不是 SSR，以及觸控區與導航形式的取捨。
2. **Jamstack 與 Edge 的分工**
   靜態內容走 CDN、狀態走 Worker + D1、離線走 localStorage。可以談三者邊界劃在哪、補送機制怎麼避免覆蓋。
3. **設計系統與規範文件**
   獨立寫了 `DESIGN.md` 與 `PRODUCT.md`，色階、字體到 WCAG 對比都有明文。可以談「朱印紅 ≤10% 面積」這類硬性約束為什麼有用。
4. **CI/CD 與資料格式守門**
   commit 觸發建置、Zod 檢核 Frontmatter、Vitest 覆蓋 store 與 Worker API。資料源是人手寫的 Markdown，這層檢核是必要的而非裝飾。

---
*索引：[[作品集總覽]]*
