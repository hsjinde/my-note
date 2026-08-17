---
title: Quartz 閱讀網站
tags: [專案, Quartz, Obsidian, 網站, GitHub-Pages, 已取代]
updated: 2026-08-17
source_count: 1
---

# Quartz 閱讀網站（my-note 公開站）

[[Ethan]] 把本 vault 發布為公開閱讀站的**初期規劃**：用 **Quartz 4**（專為發布 Obsidian vault 的靜態網站產生器）產成靜態網站部署到 GitHub Pages（`hsjinde.github.io/my-note`）。

> **狀態：已被 [[my-note-web]] 取代（2026-08）**。實際落地時沒有採用 Quartz + GitHub Pages，改為自建 Cloudflare Workers + Hono 平台（`note.19980803.xyz`），因為需要 Quartz 無法提供的能力：Webhook 增量同步、網頁端編輯回寫 Git、邊緣 AI 問答。本頁保留當初的決策脈絡作為對照——**白名單發布範圍的設計原則被 [[my-note-web]] 完整沿用**。

## 決策摘要

- **建站工具**：Quartz 4；**部署**：GitHub Pages 同一 public repo + GitHub Actions 自動 build。
- **公開範圍（白名單）**：`個人學習/`、`好工具推薦/`、`工作專案/`。
- **排除**：`wiki/`、`日常/`、`Clippings/` 不上站（repo 仍 public）。採**白名單**而非黑名單，日後新增私人資料夾不會誤外流。
- **風格**：簡約文件風（白底、無襯線 Noto Sans TC、藍點綴、大留白），Quartz 內建亮/暗切換。

## 架構

Quartz 放 repo 子資料夾 `website/`；Actions 只把白名單資料夾複製進 `website/content/` → `quartz build` → 部署 Pages。原生支援 `[[wikilinks]]`、`[!callout]`、frontmatter、中文全文搜尋、backlinks、graph。

## 已知取捨

- **跨到 `wiki/` 的 wikilink 會斷**（wiki 不上站），可接受、日後選擇性補頁。
- **`.canvas` 不渲染**（如 SRE 學習路徑圖），v1 先略過。
- 敏感掃描確認 `工作專案/` 的 `API_KEY` 等皆為文件佔位符（`<你的 API Key>`）非真實金鑰 → 可發布。

## 相關

- [[my-note-web]] — 本規劃的實際落地版本（改走 Cloudflare Workers 自建）
- [[Ethan]] — 專案作者
- [[Obsidian]] — 內容來源平台
- [[Postfix-Manager]]、[[wiki/entities/CORE-PULSE|CORE-PULSE]]、[[wiki/entities/KeyLogger-Server|KeyLogger-Server]] — Ethan 的其他專案

## 來源

- [[工作專案/Quartz 閱讀網站建置規格]]（工作紀錄，建站規格與白名單設計）
