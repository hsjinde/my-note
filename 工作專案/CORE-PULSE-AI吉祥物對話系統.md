# CORE PULSE — AI 吉祥物對話系統

> 在個人品牌網站加入 AI 吉祥物，訪客可點擊對話，回答基於 LLM Wiki 的「關於我」問題。

---

## 專案概述

在 CORE PULSE 網站（`core-pulse.19980803.xyz`）右上角增加一個 Lottie/SVG 吉祥物浮動按鈕，訪客點擊後展開聊天窗，可與 AI 對話。吉祥物以 **hsjinde 本人第一人稱** 回答，內容來自 `src/content/wiki/*.md` 中的 LLM Wiki。

### 技術棧

| 層級 | 技術 |
|------|------|
| 前端 Widget | React 19 + TypeScript 5 + Vite 5 + Framer Motion |
| 聊天 UI | react-markdown + SSE 串流 + sessionStorage 記憶 |
| LLM Client | 瀏覽器端直接呼叫 OpenAI-compatible API |
| 部署 | Cloudflare Pages + D1（rate limiting） |
| LLM 端點 | 自架 `cli.19980803.xyz/v1`（OpenAI-compatible proxy） |

---

## 系統架構

```
┌─────────────────────────────────────────────┐
│ 瀏覽器 (core-pulse.19980803.xyz)             │
│  ┌───────────────────────────────────────┐  │
│  │ <MascotWidget/>                       │  │
│  │  ├─ MascotAvatar  (Lottie/SVG 動畫)   │  │
│  │  └─ MascotChatPanel                   │  │
│  │      ├─ MessageBubble (Markdown + SSE)│  │
│  │      └─ Input (textarea + 送出/停止)   │  │
│  │                                       │  │
│  │  chatClient.ts                        │  │
│  │   ├─ 組裝 system prompt (wiki + 人格)  │  │
│  │   └─ 直接打 LLM endpoint (SSE stream)  │  │
│  └───────────────────────────────────────┘  │
│                       │                      │
│   sessionStorage      │  直接 HTTPS          │
│   (6 輪記憶)           │  (Authorization)    │
│                       ▼                      │
│              ┌──────────────────┐            │
│              │ cli.19980803.xyz │            │
│              │ /v1/chat/        │            │
│              │ completions       │            │
│              └──────────────────┘            │
└─────────────────────────────────────────────┘
```

### 關鍵設計決策

| 決策 | 原因 |
|------|------|
| 瀏覽器直接打 LLM | CF Pages Function 的 outbound IP 被自架 endpoint 擋掉 |
| CSP `connect-src` 加例外 | 預設 `'self'` 會擋掉跨域 fetch |
| Wiki build-time inline | 不改 wiki 時不做網路請求，速度快 |
| sessionStorage 6 輪記憶 | 不跨裝置，重整即清，隱私最乾淨 |
| 無後端 LLM 呼叫 | 簡化架構，LLM 請求來自使用者 IP |

---

## 文件結構

```
src/
├── components/Mascot/
│   ├── MascotWidget.tsx       # 根容器，掛 App.tsx
│   ├── MascotAvatar.tsx       # Lottie/SVG 吉祥物本體
│   ├── MascotChatPanel.tsx    # 聊天窗 UI
│   ├── MessageBubble.tsx      # 單條訊息 + Markdown
│   ├── MascotAvatar.css       # 吉祥物樣式
│   └── mascot.types.ts        # 共用型別
├── hooks/
│   └── useMascotChat.ts       # 聊天狀態 hook
├── services/
│   ├── chatClient.ts          # LLM API client (瀏覽器直連)
│   ├── prompt.ts              # System prompt 組裝
│   ├── wiki-content.ts        # Wiki 內容 client-side inline
│   └── llm-config.ts          # 自動生成: LLM endpoint config
├── content/wiki/
│   ├── identity.md            # 我的身份
│   ├── skills.md              # 技術棧
│   ├── experience.md          # 經歷
│   ├── projects.md            # 專案
│   ├── philosophy.md          # 工作哲學
│   └── contact.md             # 聯絡方式
├── App.tsx                    # 掛 <MascotWidget/>
└── index.css                  # mascot-blink keyframe

functions/api/
├── chat.ts                    # Pages Function (config + debug)
├── chat-shared.ts             # 共用型別、CORS、常數
├── chat-wiki.ts               # Server-side wiki inline
├── chat-prompts.ts            # IDENTITY_PROMPT + GUARDRAILS
├── chat-sanitizer.ts          # 輸入過濾
├── chat-rate-limit.ts         # D1 IP 限流
└── chat-llm-openai.ts         # OpenAI stream provider

scripts/
├── gen-wiki.cjs               # 生成 functions/api/_wiki-gen.ts
├── gen-client-config.cjs      # 生成 src/services/llm-config.ts
├── apply-chat-schema.sql      # D1 chat_rate_limits migration
└── set-pages-env.cjs          # Cloudflare Pages env vars 設定

public/
└── _headers                   # CSP 設定 (connect-src 白名單)

e2e/
└── mascot.spec.ts             # Playwright E2E 測試
```

---

## 環境設定

### 1. 本機開發 `.dev.vars`

```bash
LLM_API_KEY=<你的 API Key>
LLM_BASE_URL=https://cli.19980803.xyz/v1/chat/completions
LLM_PROVIDER=openai
LLM_MODEL=gpt-5.4-mini
RATE_LIMIT_SALT=cp-mascot-salt-2026-dev
RATE_LIMIT_DAILY=30
WIKI_TOKEN_BUDGET=16000
TURNSTILE_ENABLED=false
```

### 2. Production Build `.env.production`

```bash
LLM_BASE_URL=https://cli.19980803.xyz/v1/chat/completions
LLM_API_KEY=<你的 API Key>
LLM_MODEL=gpt-5.4-mini
```

### 3. Cloudflare Pages 環境變數

到 **Cloudflare Dashboard → Pages → core-pulse → Settings → Environment Variables**

| 變數                  | 值                                              | 類型        |
| ------------------- | ---------------------------------------------- | --------- |
| `LLM_API_KEY`       | -                                              | Encrypt   |
| `RATE_LIMIT_SALT`   | `cp-mascot-salt-2026-dev`                      | Encrypt   |
| `LLM_BASE_URL`      | `https://cli.19980803.xyz/v1/chat/completions` | Plaintext |
| `LLM_MODEL`         | `gpt-5.4-mini`                                 | Plaintext |
| `LLM_PROVIDER`      | `openai`                                       | Plaintext |
| `RATE_LIMIT_DAILY`  | `30`                                           | Plaintext |
| `WIKI_TOKEN_BUDGET` | `16000`                                        | Plaintext |
| `TURNSTILE_ENABLED` | `false`                                        | Plaintext |

### 4. D1 Rate Limit Table

```bash
npx wrangler@3 d1 execute core-pulse-blog --remote --file=scripts/apply-chat-schema.sql
```

### 5. CSP `_headers`

`public/_headers` 必須包含：
```
connect-src 'self' https://cli.19980803.xyz;
```
否則瀏覽器會因 Content Security Policy 擋掉跨域 fetch。

---

## 開發流程

### 本機測試

```bash
# 1. 建構
npm run build

# 2. 啟動本機 server (含 API)
npx wrangler@3 pages dev dist --port 8788

# 3. 開啟 http://localhost:8788
# 4. 點右上角吉祥物 → 對話
```

### 修改 Wiki 內容

1. 編輯 `src/content/wiki/*.md`（Markdown + YAML frontmatter）
2. 執行 `npm run build`
3. 重啟 `wrangler pages dev` 或重新部署

> 注意：改 wiki 後 `gen-wiki.cjs` 和 `gen-client-config.cjs` 會自動重新生成相關檔案

### 部署到 Production

```bash
npm run build
$env:CLOUDFLARE_API_TOKEN = "你的token"
npx wrangler@3 pages deploy dist/ --project-name=core-pulse --commit-dirty=true
```

部署後 `core-pulse.19980803.xyz` 自動更新。

---

## 測試

### 單元測試

```bash
npm test            # vitest — 30 個測試
npm run test:watch  # watch 模式
```

### E2E 測試

```bash
npm run test:e2e    # playwright
```

### 手動 QA

1. 開 `core-pulse.19980803.xyz`
2. 點右上角吉祥物
3. 問「你是誰？」— 應以第一人稱回答
4. 問「做過什麼專案？」— 應提及 CORE PULSE、OpenClaw AI
5. 問「會什麼技術？」— 應提及 Cloudflare、React、TypeScript
6. 問「幫我寫一段 Vue 程式碼」— 應拒答
7. 重整頁面 → 對話歷史應清空
8. F12 → Network → 應看不到 wiki 內容在 client bundle

---

## LLM Endpoint 資訊

- **端點**：`https://cli.19980803.xyz/v1/chat/completions`
- **可用模型**：`gpt-5.4-mini`、`claude-haiku-4-5`、`deepseek-v4-pro`、`gemini-3.5-flash` 等（共 90+）
- **格式**：OpenAI-compatible API（支援 SSE streaming）
- **腳本**：
  - `scripts/test-llm-models.mjs` — 列出可用模型
  - `scripts/test-llm-chat.mjs` — 測試特定模型
  - `scripts/test-api.cjs` — 測試 `/api/chat` 端點

要換模型，改 `.dev.vars` 或 `.env.production` 中的 `LLM_MODEL` 即可。

---

## Wiki 撰寫指南

每個 `src/content/wiki/*.md` 檔案格式：

```markdown
---
title: 顯示標題
category: identity | skills | experience | projects | philosophy | contact
tags: [tag1, tag2]
sensitivity: public
---

內文 markdown（以第一人稱撰寫）
```

### 撰寫原則

- **第一人稱**：「我目前擔任...」不是「hsjinde 目前擔任...」
- **事實優先**：寫具體數字、專案名、年限
- **不寫 PII**：電話、住址、薪資永遠不寫
- **主題單一**：一個檔一個主題
- **sensitivity**：MVP 只支援 `public`

---

## 常用指令

```bash
# 列出可用 LLM 模型
node scripts/test-llm-models.mjs

# 測試特定模型
node scripts/test-llm-chat.mjs

# 測試 API（本機）
node scripts/test-api.cjs

# 測試 API（遠端）
node -e "fetch('https://5fc7e9ce.core-pulse.pages.dev/api/chat',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({messages:[{role:'user',content:'你是誰?'}]})}).then(async r=>{const t=await r.text();console.log(t.substring(0,300))})"

# 查看 Cloudflare Pages 部署狀態
node scripts/check-deploy.mjs

# 查看 Cloudflare Pages 環境變數
node scripts/check-env.mjs

# 重設 Production 環境變數
node scripts/fix-prod-vars.mjs

# 設定 Cloudflare Pages Secrets
node scripts/set-pages-env.cjs
```

---

## 疑難排解

| 錯誤訊息 | 原因 | 解法 |
|----------|------|------|
| `[錯誤：http_500]` | D1 `chat_rate_limits` 表不存在 | `npx wrangler@3 d1 execute --local --file=scripts/apply-chat-schema.sql` |
| `[錯誤：service_unavailable]` | `LLM_API_KEY` 或 `RATE_LIMIT_SALT` 未設定 | 確認 Cloudflare env vars 已設定 |
| `[錯誤：upstream_error:401]` | LLM endpoint IP 白名單擋掉 CF IP | 已解決：切換為瀏覽器端直連 |
| `[錯誤：Failed to fetch]` | CSP `connect-src` 擋掉跨域請求 | 確認 `public/_headers` 有加 `cli.19980803.xyz` |
| `[錯誤：bad_request]` | sessionStorage 殘留舊錯誤資料 | `sessionStorage.clear()` 後重整 |
| Wrangler `Node.js v22` 錯誤 | Node 版本太低 | 用 `npx wrangler@3` |
| 吉祥物沒出現 | build 沒包含 MascotWidget | 確認 `npm run build` 通過，UI 檔案無 TypeScript 錯誤 |

---

## 版本

- 建立日期：2026-07-01
- 分支：`feat/llm-wiki-mascot` → merged to `main`
- Commit range: `0c8a182` → `f2ee452`
