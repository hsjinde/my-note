---
title: Claude Code
tags: [工具, AI, 程式助理]
updated: 2026-06-04
source_count: 2
---

# Claude Code

Anthropic 推出的命令列 AI 助理，可讀取專案檔案、執行指令、管理 vault。這個 vault 的主要 AI 介面之一（搭配 [[Obsidian]] 的 [[Claudian]] 插件使用）。

## 在這個 Vault 的角色

- 透過 [[Claude-Code-Skills]] 操作 vault 與筆記
- 維護 `wiki/` 目錄的知識提煉層
- 執行 [[Ingest-工作流]]、[[LLM-Wiki]] 的查詢與健康檢查

## 為什麼選 Claude Code

- 可搭配第三方模型（避免平台綁定）
- 支援 Skills 系統擴展功能
- 可直接讀寫本地檔案
- 在 [[Obsidian]] 中透過 [[Claudian]] 整合

## 搭配這個 Vault 的 Skills

詳見 [[Claude-Code-Skills]]，其中最重要的：
- `obsidian-cli` — vault 操作主力
- `llm-wiki` — 知識庫管理（本 vault 專用）
- `obsidian-markdown` — Markdown 撰寫

## 模型選項

- Claude Sonnet 4.6（預設，平衡性能與成本）
- Claude Haiku（成本最佳化）
- Claude Opus（高品質任務）
- 第三方模型（DeepSeek、Gemini、GPT-5.5 等）

## 相關概念

- [[Claudian]] — Obsidian 內的 Claude Code 整合
- [[Claude-Code-Skills]] — 可用 Skills 清單
- [[LLM-Wiki]] — Claude Code 維護的知識庫

## 來源

- [[目前的skills]]（2026）
- [[插件安裝]]（2026-05-28）