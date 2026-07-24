---
title: CORE PULSE 技術分享 — 如何從零打造 SRE 與 AI 系統工程師的個人品牌平台
tags: [工作專案, 技術分享, CORE-PULSE, SRE, Cloudflare, Architecture, React]
date: 2026-07-24
updated: 2026-07-24
aliases:
  - CORE PULSE 技術分享
  - Core Pulse Tech Sharing
---

# CORE PULSE 技術分享 — 從零打造高效能 SRE 與 AI 系統展演平台

> **寫在前面**：這是一篇關於 CORE PULSE (`core-pulse.19980803.xyz`) 網站開發的技術架構與設計實戰分享。網站不僅是履歷展示，更是作者 (hsjinde / Ethan) 展示 SRE 工程紀律、系統架構思維與 AI 整合能力的實驗場。

---

## 💡 為什麼要自己打造？產品定位與設計哲學

許多工程師在架設個人網站時，常面臨「選用罐頭模板缺乏特色」或「過度堆疊展示性套件導致效能低下」的兩難。CORE PULSE 的核心目標是：**讓網站設計本身就是一項工程作品**，向訪客傳達「冷靜、精準、可靠與儀器感」的工程風格。

### 1. Terminal Editorial (終端機 × 印刷雜誌) 視覺美學
摒棄常見的亮色系 SaaS landing page、大圓角毛玻璃（Glassmorphic blur）與電光漸層文字，採用極簡硬核風格：
- **純深色畫布**：以接近純黑（`#050505`）為背景，全站無淺色模式。
- **髮絲線與平直卡片**：採用 `1px rgba(255,255,255,.12)` 髮絲線邊框與 `radius <= 6px` 微圓角，營造類似儀器面板的沉穩質感。
- **終端路徑語法**：全站區塊標籤放棄傳統的大寫英文標題，改採 UNIX 終端機路徑（如 `~/skills`、`~/projects`、`~/notes`、`~/telemetry`）。

### 2. 色彩即訊號 (Color as Signal)
全站唯一無語義的裝飾色只有純白（`#ffffff`），其餘色相（Hue）嚴格保留給「活的資料與分類」：
- 🟢 **綠色 (`#30d158`)**：系統健康度（Health Check）、服務 Online 狀態、CI/CD 成功指標。
- 🔵 **藍色 (`#2997ff`)**：核心架構圖表、系統專案分類。
- 🟣 **紫色 (`#bf5af2`)**：AI 代理與深度學習相關專案。
- 🟠 **橘色 (`#ff9f0a`)**：DevOps 流程與學習筆記。
- 🔴 **紅色 (`#ff453a`)**：錯誤狀態與邊界警示。

---

## 🏗️ 系統總覽與混合雲架構

網站採用前端 SPA 搭配 Cloudflare 生態系（Pages Functions + D1 + R2）與自架 VPS（Docker + Cloudflare Tunnel）的混合雲架構：

```
                              ┌──────────────────────────────────────────────┐
                              │ Cloudflare Edge Network                      │
                              │                                              │
                              │  ┌──────────────────┐  ┌──────────────────┐  │
┌───────────────────────┐     │  │ Cloudflare Pages │  │ Cloudflare D1    │  │
│ 訪客瀏覽器            │HTTPS│  │ (React 19 SPA)   │  │ (SQLite CMS)     │  │
│ core-pulse.19980803… ├────►│  └────────┬─────────┘  └────────▲─────────┘  │
└──────────┬────────────┘     │           │                     │            │
           │                  │  ┌────────▼─────────┐           │            │
           │ Direct Fetch     │  │ Pages Functions  ├───────────┘            │
           │ (SSE Stream)     │  │ (/api/posts, auth)│                        │
           ▼                  │  └──────────────────┘                        │
┌───────────────────────┐     └──────────────────────────────────────────────┘
│ LLM Proxy Endpoint    │
│ cli.19980803.xyz/v1   │
└───────────────────────┘
```

---

## 🛠️ 關鍵技術點與架構決策實戰

### 1. 前端：React 19 + Tailwind v4 + 雙模式 API 層
在單頁應用 (SPA) 中，為了同時兼顧本地極速開發（無需起後端 API）與線上真實資料同步，我們在 `src/services/api.ts` 設計了 **Dev/Prod 雙模式分流機制**：

```typescript
// src/services/api.ts 概念範例
const isProd = import.meta.env.PROD;

export const postService = {
  async getPosts() {
    if (!isProd) {
      // 開發環境：直接讀取 localStorage Mock 資料，零網路開銷
      return JSON.parse(localStorage.getItem('cp_posts') || '[]');
    }
    // 生產環境：透過 Cloudflare Pages Functions 打 D1 SQLite 獲取資料
    const res = await fetch('/api/posts');
    if (!res.ok) throw new Error('Failed to fetch posts');
    return res.json();
  }
};
```

### 2. 後端：無伺服器 CMS 與無狀態 Session 安全認證
為了不讓個人部落格需要維護龐大的 MySQL 或 Node.js 長常駐服務，文章模組採用 **Cloudflare D1（SQLite）** 搭配 **Pages Functions**：
- **文章發佈與 CRUD**：`/api/posts` 透過 SQL 操作 D1，文章內容採用 Markdown 儲存。
- **無狀態 HMAC 簽章 Session**：
  - 管理員登入成功後，後端會簽發包含 HMAC-SHA256 簽章的 `cp_session` Token，存放在 `HttpOnly / Secure / SameSite=Strict` Cookie 中。
  - 驗證時採用 **常數時間比對（Constant-time comparison）** 避免 Timed-attack 攻擊。
  - 前端受保護路由 (`ProtectedRoute`) 掛載時打 `/api/auth/check` 進行驗證，實現免重新部署的雲端 CMS 發佈體驗。

### 3. AI 對話頁面重構 (/ask) 與 Inline LLM Wiki 實作
核心對話體驗進行了關鍵重構：**移除原先右下角浮動按鈕 (Mascot Widget/Avatar)，改為獨立的 `/ask` 路由全頁面對話體驗 (`Ask.tsx`)**。訪客點擊頂部 Navbar 的 `ask` 連結即可進入獨立分頁進行對話。

> 架構與演進歷史詳見筆記：[[CORE-PULSE-AI吉祥物對話系統]]

- **獨立全頁對話視窗 (`src/pages/Ask.tsx`)**：
  - 整合 `useMascotChat` 狀態機與 `MessageBubble` 模組。
  - 提供預設問答快捷 Chip（如「核心技術棧」、「資安工作」、「自架服務」、「聯絡方式」），降低訪客提問門檻。
  - 行動端優化：透過 `window.visualViewport` 動態計算軟體鍵盤彈出高度，確保輸入框不被彈出鍵盤遮擋。
- **Build-Time Inline Wiki**：撰寫 `scripts/gen-wiki.cjs` 在建置期 (`npm run build`) 將 `src/content/wiki/*.md` 轉譯打包至 `functions/api/_wiki-gen.ts`，達成 System Prompt 0 網路延遲注入。
- **瀏覽器直連 LLM (Client-Side Streaming)**：瀏覽器端直接透過 Fetch SSE 呼叫自架 LLM 端點 (`cli.19980803.xyz/v1`)，並在 `public/_headers` 設定 CSP 白名單 (`connect-src 'self' https://cli.19980803.xyz;`)。
- **隱私機制**：使用 `sessionStorage` 維持 6 輪對話記憶，頁面關閉或重整時隨即自動抹除，確保隱私乾淨。

### 4. Telemetry 與系統可觀測性 (Observability)
作為 SRE 的個人網站，即時可觀測性是不可或缺的細節：
- **LCP 即時檢測**：利用 `PerformanceObserver` 監控訪客載入時的最大內容繪製時間 (LCP)，並即時渲染於 Footer。
- **Build Time 標籤**：透過 Vite `vite.config.ts` 的 `define` 設定，將 `__BUILD_TIME__` 在打包瞬間注入程式碼。
- **Health Check 心跳點**：Footer 的健康燈即時打 `/api/health` 端點，以綠燈（`#30d158`）或紅燈呈現後端 Functions 存取狀態。

---

## 🧪 品質保證與 CI/CD 流程

為確保程式碼品質與防範重構引入 Bug，專案建立嚴格的自動化測試防線：

```
[ Git Push to main ]
        │
        ├──► 1. 型別檢查：npx tsc --noEmit
        ├──► 2. 單元測試：Vitest (測試 API、Prompt 組裝與狀態機)
        ├──► 3. E2E 測試：Playwright (模擬訪客點擊 ask 頁面與 Admin 登入)
        └──► 4. 自動部署：GitHub Actions 呼叫 Wrangler v3 發佈至 Cloudflare Pages
```

---

## 📈 經驗總結與採坑紀錄 (Key Learnings)

1. **從浮動 Widget 重構至全頁對話 (`/ask`) 的 UI 體驗升級**：
   舊版浮動視窗在手機版容易遮擋網頁內容，且輸入框容易被鍵盤蓋住；改為獨立路由 `/ask` 後，搭配 `visualViewport` API 處理高度，帶來更乾淨、專注且行動裝置友善的聊天體驗。
2. **CSP (Content Security Policy) 踩坑**：
   在將 LLM Fetch 移至瀏覽器直連時，必須在 `public/_headers` 顯式聲明 `connect-src` 允許清單，否則瀏覽器預設 `connect-src 'self'` 會擋掉跨域請求。
3. **Tailwind CSS v4 升級體驗**：
   Tailwind v4 取消 `tailwind.config.js`，改在 `index.css` 以 `@theme` 定義 Design Tokens，大幅簡化自訂顏色與字體變數的維護成本。

---

## 🔗 相關資源與延伸閱讀

- 專案線上展示：[CORE PULSE](https://core-pulse.19980803.xyz)
- 核心對話系統筆記：[[CORE-PULSE-AI吉祥物對話系統]]
- 相關工具與 Skill 筆記：[[自製 Claude Code Skills 總覽]]、[[Claude Code Skills 安裝指南]]
