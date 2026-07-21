---
title: CORE PULSE
tags: [專案, AI, 前端, Cloudflare, LLM-Wiki]
updated: 2026-07-21
source_count: 1
---

# CORE PULSE — AI 吉祥物對話系統

[[Ethan]] 個人品牌網站（`core-pulse.19980803.xyz`）上的 AI 吉祥物：右下角 SVG 企鵝浮動按鈕，訪客點擊展開聊天窗，以 **hsjinde 本人第一人稱**回答「關於我」的問題，內容來自站內的一份 LLM Wiki。是 [[LLM-Wiki]] 概念的一個對外應用。

## 技術棧

| 層 | 技術 |
|---|---|
| 前端 Widget | React 19 + TypeScript + Vite + Framer Motion |
| 聊天 UI | react-markdown + SSE 串流 + sessionStorage（6 輪記憶，重整即清） |
| LLM Client | 瀏覽器端**直接**呼叫 OpenAI-compatible API |
| 部署 | Cloudflare Pages + D1（IP rate limiting） |
| LLM 端點 | 自架 OpenAI-compatible proxy（90+ 模型可選） |

## 關鍵設計決策

- **瀏覽器直接打 LLM**：因 CF Pages Function 的 outbound IP 被自架 endpoint 擋掉；改由使用者 IP 直連（需在 CSP `connect-src` 加白名單例外）。
- **Wiki build-time inline**：不改 wiki 時不做網路請求，速度快；wiki 內容不進 client bundle。
- **sessionStorage 記憶**：不跨裝置、重整即清，隱私最乾淨。
- **無後端 LLM 呼叫**：簡化架構。

## 站內 Wiki 與 Guardrails

`src/content/wiki/*.md` 以第一人稱分主題（identity / skills / experience / projects / philosophy / contact），只寫 `sensitivity: public`、絕不寫 PII（電話、住址、薪資）。有 guardrails：問「你是誰」以第一人稱答、問專案提及 CORE PULSE / OpenClaw，問「幫我寫 Vue」則拒答。

## 相關

- [[Ethan]] — 網站主人（同一 `core-pulse.19980803.xyz` 站點即他的 [[Clippings/職涯/SRE Engineer & AI Systems Developer|SRE 作品集]]）
- [[LLM-Wiki]] — 吉祥物回答的知識來源模式
- [[Postfix-Manager]]、[[KeyLogger-Server]]、[[Quartz-閱讀網站]] — Ethan 的其他專案

## 來源

- [[工作專案/CORE-PULSE-AI吉祥物對話系統]]（工作紀錄，系統架構與部署）
