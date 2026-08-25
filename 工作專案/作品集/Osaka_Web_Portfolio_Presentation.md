---
title: 大阪旅券 OSAKA TRIP — 和風手帳設計的全棧旅遊儀表板（作品集簡報）
tags: [作品集, 工作專案, Osaka-web, React, Cloudflare, Jamstack, 設計系統, 求職]
date: 2026-08-13
updated: 2026-08-25
---

# 大阪旅券 OSAKA TRIP — 離線可用、和風手帳設計的全棧旅遊儀表板

> 以 Obsidian Markdown 為資料來源的 Jamstack 應用：CI 建置成靜態站，收藏與待辦狀態存在 Cloudflare D1，斷線時退回 localStorage、連線後補送。
>
> 這是為了一趟真的要去的家族旅行做的，不是練習題。

---

## 求職摘要

* **求職目標**：Senior Frontend Engineer / Full-Stack Web Architect / Jamstack Systems Specialist / UI/UX Systems Developer
* **線上環境**：[osaka.19980803.xyz](https://osaka.19980803.xyz)（Cloudflare Pages 自訂網域）｜備援 [GitHub Pages](https://hsjinde.github.io/Osaka-web/) 雙部署
* **儲存庫**：[hsjinde/Osaka-web](https://github.com/hsjinde/Osaka-web)（前端與 Worker）｜[hsjinde/Osaka-vault](https://github.com/hsjinde/Osaka-vault)（Obsidian 資料來源）
* **技術棧**：React 18、TypeScript、Vite、Tailwind CSS、Cloudflare Worker (Hono)、Cloudflare D1、Zod、GitHub Actions、Vitest、Playwright
* **關鍵數字**（2026-08-25 實跑確認）
  * **36 個 Vitest 測試檔、188 個測試全過**
  * 主色朱印紅 `#B23A1E` 的畫面填色面積硬性上限 **≤10%**
  * 正文對比 ≥4.5:1（WCAG AA）、觸控目標 ≥36–44px
  * 資料來源是人手寫的 Markdown，格式錯誤在 CI 階段就被 Zod 擋下，進不了生產環境

---

## 為什麼做這個

多人家族旅行，真的踩過的三個痛點：

1. **資訊散落**。餐廳攻略、景點網址、飯店確認單分散在 LINE 群組跟各自的筆記裡。要找某家店的營業時間，得往上滑三百則訊息。
2. **弱網環境**。人在海外，行動網路時好時壞。載入要快，最好斷線也還能看。
3. **跨裝置與多人**。行程進度跟待辦勾選要即時同步，但又不想讓隨手瀏覽的人改到主清單。

解法是把 Obsidian Markdown 當單一資料來源，CI 建置成靜態站；狀態交給 Cloudflare Worker + D1，做到不用刷新、離線可用、讀寫權限分離。

順帶一提，這也是我第二套自建設計系統。CORE PULSE 是黑底儀器感，這邊是和紙與朱印——刻意選一個完全相反的方向，證明那套視覺紀律不是只會做一種風格。

---

## 線上畫面

### 1. 桌機端首屏

![Osaka Web 桌機端首屏畫面](screenshots/osaka_web_live_desktop_hero.png)

不做 SaaS/Notion 那種中性樣板，品牌意象是「御朱印帳」。和紙紋理底色 `#F1EBDD`、襯線標題 Shippori Mincho、朱印紅 `#B23A1E`。

### 2. 行動端

![Osaka Web 行動端介面展示](screenshots/osaka_web_live_mobile_hero.png)

設計情境很具體：站在大阪街頭、單手拿手機、網路時好時壞。所以是膠囊形導航列、大字級對比、微動畫回饋，觸控區 ≥36–44px。這幾個數字不是抄規範抄來的，是想像自己戴著手套、單手滑手機的時候會不會點錯。

### 3. 桌機全頁

![Osaka Web 桌機全頁面展示](screenshots/osaka_web_live_desktop_full.png)

### 4. 行動端全頁

![Osaka Web 行動端全頁面展示](screenshots/osaka_web_live_mobile_full.png)

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

北極星是「大阪旅券・御朱印帳」。但意象講得再美，寫 code 的時候也用不上，所以拆成四條可執行的規則：

* **字體雙軌**：標題與大數字用襯線 `Shippori Mincho` 撐儀式感，正文與 UI 標籤用 `Noto Sans TC` 顧可讀性。
* **One Seal Rule**：朱印紅 `#B23A1E` 是唯一主色，畫面填色面積硬性限制 **≤10%**，只給主行動、當前分頁跟重要警示。
* **No-Grey Rule**：不用低飽和中性灰，改用墨色 `#29231A` 與暖茶梯度，配 1.8% 橫向紙紋。
* **狀態互動**：蓋章進場 `stampIn`、愛心彈跳 `heartPop`、同步呼吸 `sealPulse`，全部支援 `prefers-reduced-motion` 降級。

「≤10%」這種硬性數字很好用——它把「這裡要不要加個紅色按鈕」從品味問題變成算術問題。

### 2. 離線韌性狀態機

讀寫分離：訪客免密碼就能讀到 D1 裡的最新同步狀態；設定通行密碼之後瀏覽器持有 Bearer Token，才能 `PUT` 更新。

旅途中連線中斷、或 Worker 連不上的時候，狀態先寫進 `localStorage`，恢復連線後自動補送。

這段是為了現場真的會發生的事寫的，不是為了 demo。地鐵站裡沒訊號、飯店 Wi-Fi 卡住，這些都不是假設。

### 3. Zod 當建置期守門員

資料源是人手寫的 Markdown，格式一定會有人寫錯——包括我自己。

`scripts/build-data.ts` 在建置時解析所有 Frontmatter 與正文，欄位不符 Zod schema（日期不是 `YYYY-MM-DD`、缺必填欄位）就讓 CI 拋錯並指出行號。格式錯誤進不到生產環境。

不加這層的話，錯誤會變成線上頁面上一個空白區塊，而且要等到有人真的去點才會發現。

### 4. 行程結構的 GitHub API 回寫

收藏與待辦記在 D1；但在前端調整每日行程結構時，Worker 會透過 GitHub REST API 以 Base64 加 SHA 防衝突機制置換 Markdown 區段（`spliceItinerary`）並 commit 回儲存庫，自動觸發重新部署。

也就是說行程的真值永遠留在 Markdown 裡，網頁只是一個比較好用的編輯器。這樣就算網站整個掛掉，行程還在 Obsidian 裡看得到。

---

## 測試與品質

2026-08-25 實跑：

```
 Test Files  36 passed (36)
      Tests  188 passed (188)
   Duration  12.51s
```

* **前端 store 與 parser**：覆蓋 Markdown 資料剖析、倒數計時、狀態轉換與動態效果。
* **Worker API**：驗證 Hono 路由、Bearer 權限與 D1 SQL 操作。
* **無障礙**：正文對比 ≥4.5:1（WCAG AA），觸控目標 ≥36–44px。

---

## 面試問答

### Q1：為什麼選 SSG 而不是 SSR？

> 「因為使用情境反過來決定了架構。
> 這是人在關西街頭、單手拿手機、網路時好時壞的時候在用的東西。SSR 每次都要等伺服器回應，弱網下就是白畫面；SSG 直接從 CDN 拿靜態檔，載入完就在那裡了。
> 需要即時的只有『誰勾了哪個待辦』這件事，那部分才走 Worker + D1。內容靜態、狀態動態，界線劃在這裡。」

### Q2：Jamstack、Edge 與離線三層怎麼分工？

> 「靜態內容走 CDN，狀態走 Worker + D1，離線走 localStorage。
> 關鍵是補送機制不能造成覆蓋——所以是讀寫分離：訪客免認證讀 D1 的最新狀態，寫入要 Bearer Token。斷線期間的變更先落在 localStorage，恢復連線後才送出去。
> 這樣『隨手瀏覽的親戚』不會不小心改掉主清單，但他隨時看得到最新進度。」

### Q3：設計系統為什麼要寫成文件？

> 「我另外寫了 `DESIGN.md` 跟 `PRODUCT.md`，色階、字體到 WCAG 對比都有明文。
> 有用的不是文件本身，是那幾條硬性約束。像『朱印紅填色面積 ≤10%』——沒有這條的話，每個新元件都會有人（就是我）想說『這裡加個紅色會比較醒目』，加著加著整頁都是紅的，主色就不再是訊號了。
> 寫成數字之後，這件事就不用每次重新討論。」

### Q4：資料源是人手寫的 Markdown，怎麼保證品質？

> 「commit 觸發建置、Zod 檢核 Frontmatter、Vitest 覆蓋 store 與 Worker API，目前 36 個測試檔、188 個測試。
> Zod 那層是必要的而不是裝飾——日期少寫一位數、少一個必填欄位，如果沒擋，就會變成線上一塊空白，然後在大阪路邊才發現。CI 擋下來只花我三十秒。」

---

## 履歷描述範本

### 繁體中文

* **設計與開發跨裝置同步之 Jamstack 旅遊儀表板 (Osaka Trip)**：以 React 18、TypeScript 與 Vite 建構前端，將 Obsidian Markdown 作為單一資料來源經 GitHub Actions 建置為靜態站，同步部署至 Cloudflare Pages 自訂網域與 GitHub Pages 雙環境。
* **實作離線韌性狀態機與讀寫權限分離**：訪客免認證即可讀取 Cloudflare D1 中的同步狀態，寫入則需 Bearer Token 授權；連線中斷時狀態自動落於 `localStorage`，恢復連線後補送，確保海外弱網環境下不中斷使用。
* **建立建置期型別守門與雙向內容回寫**：以 Zod Schema 於 CI 階段驗證所有 Markdown Frontmatter 並回報錯誤行號，阻擋格式錯誤進入生產環境；前端調整行程結構時由 Cloudflare Worker 透過 GitHub REST API 以 Base64 與 SHA 防衝突機制置換 Markdown 區段並自動觸發重新部署。
* **自建和風編輯設計系統與無障礙規範**：撰寫 `DESIGN.md` / `PRODUCT.md` 規範，制定襯線／無襯線字體雙軌、主色填色面積 ≤10% 等硬性約束，達成 WCAG AA 對比（正文 ≥4.5:1）與 ≥36–44px 觸控目標，並支援 `prefers-reduced-motion` 降級；品質由 36 個 Vitest 測試檔、188 項測試守護。

### English

* **Designed and Shipped a Cross-Device Jamstack Travel Dashboard (Osaka Trip)**: Built with React 18, TypeScript and Vite, using Obsidian Markdown as the single source of truth compiled by GitHub Actions into a static site, dual-deployed to a Cloudflare Pages custom domain and GitHub Pages.
* **Implemented an Offline-Resilient State Engine with Split Read/Write Access**: Visitors read synced state from Cloudflare D1 without authentication while writes require a Bearer token; state falls back to `localStorage` when connectivity drops and replays on reconnect, keeping the app usable on unreliable overseas networks.
* **Established Build-Time Type Gating and Bidirectional Content Writeback**: Validated every Markdown frontmatter against Zod schemas in CI with precise line-number reporting so malformed data never reaches production; itinerary edits made in the UI are spliced back into Markdown by a Cloudflare Worker via the GitHub REST API using base64 and SHA conflict guards, auto-triggering redeployment.
* **Authored a Washi Editorial Design System with Accessibility Constraints**: Wrote `DESIGN.md` / `PRODUCT.md` specifications defining a serif/sans dual-track type system and a hard ≤10% accent-fill budget, meeting WCAG AA contrast (≥4.5:1 body text) and ≥36–44px touch targets, with full `prefers-reduced-motion` degradation, backed by 188 Vitest tests across 36 suites.

---
*索引：[[作品集總覽]]*
